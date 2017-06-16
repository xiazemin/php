# TSRMLS\_CC

术语



TSRM



Thread Safe Resource Manager - 这是一个经常被忽视的层面，就会有也是很少被讨论到，在你的PHP源代码包中，它被隐藏在/TSRM目录中。默认情况下，TSRM 层只在编译需要它的SAPI的时候才会打开（例如apache2-worker）。所有的在win32下编译的SAPI都会有TSRM层而不会管它们是否需要。



ZTS



Zend Thread Ssafety -通常情况下，与TSRM有相同的用处。具体来说不同在于，ZTS的使用需要在编译的时候加上参数如./configure \( --enable-experimental-zts for PHP4, --enable-maintainer-zts for PHP5），而TSRM是否被使用是由\#define的预处理机制来确定的。



tsrm\_ls



TSRM local storage - 当ZTS可用时，这是它在TSRMLS\_\*宏里传来传去的实际的变量名。它作为一个指向那个线程的独立数据存储块开始位置的指针，我会在一会儿后将它覆盖掉。



TSRMLS\_??



有四个宏设计来为了使ZTS模式和非ZTS模式之间的差异变得平滑，当ZTS不能用时，这四个宏同时都没有任何用处。而在ZTS启用的时候，它们扩大了以下的定义：



TSRMLS\_C   tsrm\_ls

TSRMLS\_D   void \*\*\*tsrm\_ls

TSRMLS\_CC  , tsrm\_ls

TSRMLS\_DC  , void \*\*\*tsrm\_ls

综述



在任何普通的C程序（PHP也一样）中，要在两个不同的函数间传递相同的数据块，你两种办法。



第一种方法是通过函数的参数传递值：



\#include &lt;stdio.h&gt;



void output\_func\(char \*message\)

{

    printf\("%sn", message\);

}



int main\(int argc, char \*argv\[\]\)

{

    output\_func\(argv\[0\]\);



    return 0;

}

另一种方法，你可以把这个值保存在一个上层的全局变量中，让函数通过它来取得：

\#include &lt;stdio.h&gt;



char \*message;



void output\_func\(void\)

{

    printf\("%sn", message\);

}



int main\(int argv, char \*argv\[\]\)

{

    message = argv\[0\];

    output\_func\(\);



    return 0;

}

这两种方法各有优点和缺点，而且在在实际应用中你常常会看到两者结合使用。事实上，PHP里到处都是全局变量，从类型标识符，到函数回调指针，再到请求特殊的信息例如用于存储用户空间变量的符号表。试图通过函数参数的方法来传递那些变量是不方便的，例如PHP这样的应用常常会需要去注册回调方法来给不支持上下文数据的外部的库使用，此时使用参数传递是不可能的。



因此，共同的信息，像执行栈、函数表、类表以及扩展登记表都是在全局范围里进行声明的，以便它们能在整个应用的任何一个位置都能使用。对于单线程的SAPIs，例如CLI、APACHE1，或者甚至Apache2-prefork，这样做都是完美的。在RINIT/Activation 阶段，请求具体结构被初始化，并在RSHUTDOWN/Deactivation 阶段重新设置回初始的值等待下一次的请求。一个像APACHE1这样特定的WEB服务器，可以一次处理多个页面，是因为它会产生多个进程，每个进程会在自己的进程空间内使用自己的全局变量副本。



现在，让我们来介绍像Apache2-worker或者IIS这样的线程服务器。在这种情况下，在指定时间里只有一个进程空间是可用的，由它抛出多线程处理。每一个线程都会类似一个单独的进程的方式工作。在处理请求的时候服务会一次一个地来。当两个或者多个线程去处理同一时间的一个请求的时候，麻烦就出现了。每个线程都试图去使用全局变量来保存它的特定请求信息，并尝试写向同样的存储空间。这样子最少会导致一个脚本里声明的用户空间变量在另一个脚本中出现。而在实践中，这样会导致快速灾难性的错误和不可预测的情况，因为内存可以释放两次或者独立的线程写了相互矛盾的信息。



不全的全局



解决方案需要一个引擎、一个核心，还有任何一个使用全局存储的扩展都要决定会使用多少内存给具体的请求数据。然后，在运行的每一个新的线程中，给它们每个分配一块内存，用来存储它们的数据，从而使每个线程都有自己的本地存储。为了把所有的被指定线程用到的内存块聚成一组，最后一个指针向量被开辟来保存子结构的指针。这个指针指向tsrm\_ls变量，被TSRMLS\_\*家族的宏所使用。为了看清它是怎么工作的，我们先来看一个扩展的例子：



typedef struct \_zend\_myextension\_globals {

    int foo;

    char \*bar;

} zend\_myextension\_globals;



\#ifdef ZTS

int myextension\_globals\_id;

\#else

zend\_myextension\_globals myextension\_globals;

\#endif



/\* Triggered at the beginning of a thread \*/

static void php\_myextension\_globals\_ctor\(zend\_myextension\_globals \*myext\_globals TSRMLS\_DC\)

{

    myext\_globals-&gt;foo = 0;

    myext\_globals-&gt;bar = NULL;

}



/\* Triggered at the end of a thread \*/

static void php\_myextension\_globals\_dtor\(zend\_myextension\_globals \*myext\_globals TSRMLS\_DC\)

{

    if \(myext\_globals-&gt;bar\) {

        efree\(myext\_globals-&gt;bar\);

    }

}



PHP\_MINIT\_FUNCTION\(myextension\)

{

\#ifdef ZTS

    ts\_allocate\_id\(&myextension\_globals\_id, sizeof\(zend\_myextension\_globals\),

                   php\_myextension\_globals\_ctor, php\_myextension\_globals\_dtor\);

\#else

    php\_myextension\_globals\_ctor\(&myextension\_globals TSRMLS\_CC\);

\#endif



    return SUCCESS;

}



PHP\_MSHUTDOWN\_FUNCTION\(myextension\)

{

\#ifndef ZTS

    php\_myextension\_globals\_dtor\(&myextension\_globals TSRMLS\_CC\);

\#endif



    return SUCCESS;

}

这里可以看到，扩展在启动的时候声明了它全局需求的变量给TSRM层，它需要sizeof\(zend\_myextension\_globals\)字节的存储，并且在初始化或者销毁指定的线程的本地存储的时候提供了回调函数。myextension\_globals\_id里的值可得出偏移量（对所有线程是公共的），存储于tsrm\_ls向量，指向那个线程本地存储的指针中也是这个向量。如果ZTS不能用，数据存储只不过是放在真实的全局环境中，并且在线程初始和销毁的过程中需要手动执行初始化和销毁的程序。如果你现在想知道为什么TSRMLS\_CC 被包含在非ZTS块中，那我可以很确定我没有使你看睡着了。这些都是不是必须的，因为我们知道非ZTS里他们有和没有一个样，但这有助于鼓励函数原型调用时引用它们的好习惯。



把它们聚在一起



最后一块线程安全的疑惑来自于一个问题：“我如何获得这些数据结构？”这个问题的答案来自于另一个有着相似样子的宏。在每个扩展或者是核心组件定义的头文件中，有一个宏的定义如下所示：



\#ifdef ZTS

\# define   MYEXTENSION\_G\(v\)     

             \(\(\(zend\_myextension\_globals\*\)\(\*\(\(void \*\*\*\)tsrm\_ls\)\)\[\(myextension\_globals\_id\)-1\]\)-&gt;v\)

\#else

\# define   MYEXTENSION\_G\(v\)     \(myextension\_globals.v\)

\#endif

因此，当ZTS不可用时，这个宏简单地收集来自全局环境中的即时值作用正确的值，否则，它将根据ID将结构开辟在本线程的本地存储拷贝中，并且从那里引用到值。

