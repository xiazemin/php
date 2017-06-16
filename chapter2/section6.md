# PEAR

PEAR就象CPAN对于PERL一样，会让PHP产生更高的能量。

什么是ＰＥＡＲ

ＰＥＡＲ是ＰＨＰ扩展与应用库（the PHP Extension and Application Repository）的缩写。它是一个ＰＨＰ扩展及应用的一个代码仓库，简单地说，ＰＥＡＲ就是ＰＨＰ的ＣＰＡＮ。

为什么要使用ＰＥＡＲ？

PHP是一个非常优秀的脚本语言，简洁、高效，随着4.0的发布，越来越多的人使用它来进行动态网站的开发，可以说，PHP已经成为最优秀的INTERNET开发语言之一，尤其对于那些需要能够快速、高效地开发中小规模的商业应用的网站开发人员，PHP是其首选的语言。但是随着PHP的应用的不断增多，对于这些应用缺乏统一的标准和有效的管理，因此，PHP社区很难象PERL社区的人们那样方便的共享彼此的代码和应用，因为PHP缺乏象CPAN那样的统一的代码库来分类管理应用的代码模块（熟悉PERL的人都知道，CPAN是一个巨大的PERL的扩展模块仓库，编写的应用模块可以放在CPAN下面的适当的分类目录下面，其他的人可以很方便地复用，当然，你编写应用模块时候也需要遵守其中的准则。）

为此，PEAR就应运而生了，并且从4.04开始，随着PHP核心一起被分发。

ＰＥＡＲ能给我带来什么好处？

1.如前所述，PEAR按照一定的分类来管理PEAR应用代码库，你的PEAR代码可以组织到其中适当的目录中，其他的人可以方便地检索并分享到你的成果。

2.PEAR不仅仅是一个代码仓库，它同时也是一个标准，使用这个标准来书写你的PHP代码，将会增强你的程序的可读性，复用性，减少出错的几率。

3.PEAR通过提供2个类为你搭建了一个框架，实现了诸如析构函数，错误捕获功能，你通过继承就可以使用这些功能。

ＰＥＡＲ的编码规则

PEAR的编码规则包括缩进规则，控制结构，函数调用，函数定义，注释，包含代码，PHP标记，文件头的注释块，CVS标记，URL样例，常量的命名这11方面。下面简要地介绍一下：

缩进规则：

PEAR中需要使用4个空格来缩排代码，并且不使用TAB。如果你使用VIM，将下列设置放入你的~/.vimrc中：

set expandtab

set shiftwidth=4

set tabstop=4

如果，你使用Emacs/XEmacs,需要把indent-tabs-mode 设置成nil。

不过你象我一样喜欢用\(X\)Emacs编辑PHP文件，我强烈推荐你安装PHP-MODE，这样当你编写PEAR代码的时候，它会自动调整你的缩排风格，当然PHP-MODE还有许多很优秀的特性，你可以从资源列表中的地方下载最新版的PHP-MODE。

控制结构：

这里所说的控制结构包括: if for while switch 等。对于控制结构，在关键字（如if for ..）后面要空一个格，然后再跟控制的圆括号，这样，不至于和函数调用混淆，此外，你应该尽量完整的使用花括号{}，即使从语法上来说是可选的。这样可以防止你以后需添加新的代码行时产生逻辑上的疑惑或者错误。这里是一个样例：

if \(\(条件1\) && \(条件2\)\) {

    语句1；

}esleif \(\(条件3\) \|\| \(条件4\)\) {

    语句2；

}else {

    语句3；

}

函数调用：

对于函数调用，函数名和左括号\( 之间不应该有空格，对于函数参数，在分隔的逗号和下一个参数之间要有相同的空格分离，最后一个参数和右括号之间不能有空格。下面是一个标准的函数调用；

$result = foo\($param1, $param2, $param3\);

不规范的写法：

$result=foo \($param1,$param2,$param3\);

$result=foo\( $param1,$param2, $param3 \);

此外，如果要将函数的返回结果赋值，那么在等号和所赋值的变量之间要有空格，同时，如果是一系列相关的赋值语句，你添加适当的空格，使它们对齐，就象这样：

$result1 = $foo\($param1, $param2, $param3\);

$var2    = $foo\($param3\);

$var3    = $foo\($param4, $param5\);

函数定义：

函数定义遵循"one true brace"习俗：

function connect\(&$dsn, $persistent = false\)

{

    if \(is\_array\($dsn\)\) {

        $dsninfo = &&dsn;

    } else {

        $dsninfo = DB::parseDSN\($dsn\);

    }

    if \(!$dsninfo \|\| !$dsninfo\['phptype'\]\) {

        return $this-&gt;raiseError\(\);

    }

    return true;

}

如上所示，可选参数要在参数表的末端，并且总是尽量返回有意义的函数值。

关于注释：

对于类的在线文档，应该能够被PHPDoc转换，就象JavaDoc那样。PHPDoc也是一个PEAR的应用程序，更详细的介绍你可以去 http://www.phpdoc.de/ 查看。除了类的在线文档，建议你应该使用非文档性质的注释来诠释你的代码，当你看到一段代码时想：哦，我想不需要在文档里去仔细描述它吧。那么你最好给这段代码作一个简单的注释，这样防止你会忘记它们是如何工作的。对于注释的形式，C的 /\* \*/和C++的//都不错，不过，不要使用Perl或者shell的\#注释方式。

包含代码：

无论什么时候，当你需要无条件包含进一个class文件，你必须使用requre\_once;当你需要条件包含进一个class文件，你必须使用include\_once;这样可以保证你要包含的文件只会包含一次，并且这2个语句共用同一个文件列表，所以你无须担心二者会混淆，一旦require\_once 包含了一个文件，include\_once不会再重复包含相同的文件，反之亦然。

PHP代码标记：

任何时候都要使用&lt;?php ?&gt;定义你的php代码，而不要简单地使用&lt;? ?&gt;,这样可以保证PEAR的兼容性，也利于跨平台的移植。

文件头的注释声明：

所有需要包含在PEAR核心发布的PHP代码文件，在文件开始的时候，你必须加入以下的注释声明：

/\* vim: set expandtab tabstop=4 shiftwidth=4: \*/

// +----------------------------------------------------------------------+

// \| PHP version 4.0                                                      \|

// +----------------------------------------------------------------------+

// \| Copyright \(c\) 1997, 1998, 1999, 2000, 2001 The PHP Group             \|

// +----------------------------------------------------------------------+

// \| This source file is subject to version 2.0 of the PHP license,       \|

// \| that is bundled with this package in the file LICENSE, and is        \|

// \| available at through the world-wide-web at                           \|

// \| http://www.php.net/license/2\_02.txt.                                 \|

// \| If you did not receive a copy of the PHP license and are unable to   \|

// \| obtain it through the world-wide-web, please send a note to          \|

// \| license@php.net so we can mail you a copy immediately.               \|

// +----------------------------------------------------------------------+

// \| Authors: Original Author                         \|

// \|          Your Name                                  \|

// +----------------------------------------------------------------------+

//

// $Id$

对于不在PEAR核心代码库中的文件，建议你也在文件的开始处有这样一个类似的注释块，标明版权，协议，作者等等。同时也在第一行加入VIM的MODELINE，这样在VIM中能够保持PEAR的代码风格。

CVS标记：

如上面所展示那样，在每个文件中加入CVS的ID标记，如果你编辑或修改的文件中没有这个标记，那么请加入，或者是替换原文件中相类似的表现形式（如"Last modified"等等）

URL样本：

你可以参照RFC 2606,使用"www.example.com"作为所有的URL样本。

常量命名：

常量应该尽量使用大写，为了便于理解，使用下划线分割每个单词。同时，你应该常量所在的包名或者是类名作为前缀。比如，对于Bug类中常量应该以Bug\_开始。以上是PEAR的编码规则，详细的编码规则可以参考PEAR中的CODING\_STANDDARD文件的说明。为了更好地理解这些编码规则，你也可以参考一下现有PEAR核心模块的代码。

回页首

开始使用ＰＥＡＲ

PEAR

使用PEAR很简单，你只需这样定义你自己的PEAR程序：

require\_once "PEAR.php";

class your\_class\_name extends PEAR{

	你的类定义...

}

当然，你需要遵守前面说的PEAR的编码规则，之后你就可以在你的类内部实现你要做的事情了。下面，我们展开讨论一下，实际上PEAR为我们提供了2个预定义类： PEAR:这是PEAR的基类，所有的PEAR扩展都要从它继承派生出来。 PEAR\_Error：PEAR的错误处理的基类，你可以选择派生出自己的错误处理的类。

一般来说，你不应该直接创建PEAR的实例，而是要自己派生出一个新的类，然后再创建这个新类的实例。作为基类，PEAR给我们提供了一些有用的功能，最主要的就是析构函数和错误处理

析构函数

PHP支持构造函数，但是并不支持析构函数，不过，PHP提供register\_shutdown\_function\(\)这个函数，从而能够在脚本终止前回调注册的函数，因此PEAR利用这个特性，提供了析构函数的仿真。假如你有一个PEAR的子类，叫做mypear,那么在mypear类中，你可以定义一个函数，函数名是下划线加上你的类名，\_mypear\(\),这个函数就是这个类的析构函数。不过这个析构函数和C++中的析构函数不太一样，它不会在对象被删除的时候执行，而是在脚本结束的时候，毕竟这只是一个仿真。由于是使用了register\_shutdown\_function\(\)，所以在你的析构函数里，打印的信息将不会返回浏览器中。此外，在你的构造函数中，需要调用一下它的父类的构造函数，因为PHP不会自动调用父类的构造函数，而析构函数需要在PEAR的构造函数中注册，我们可以看看PEAR的源代码：

&lt;code&gt;

function PEAR\(\) {

if \(method\_exists\($this, "\_".get\_class\($this\)\)\) {

    global $\_PEAR\_destructor\_object\_list;

    $\_PEAR\_destructor\_object\_list\[\] = &this;

}

if \($this-&gt;\_debug\) {

    printf\("PEAR constructor called, class=%s\n",

           get\_class\($this\)\);

}    

.....

function \_PEAR\_call\_destructors\(\) {

    global $\_PEAR\_destructor\_object\_list;

    if \(is\_array\($\_PEAR\_destructor\_object\_list\) && 

          sizeof\($\_PEAR\_destructor\_object\_list\)\) {

    reset\($\_PEAR\_destructor\_object\_list\);

    while \(list\($k, $objref\) = each\($\_PEAR\_destructor\_object\_list\)\) {

        $destructor = "\_".get\_class\($objref\);

        if \(method\_exists\($objref, $destructor\)\) {

        $objref-&gt;$destructor\(\);

        }

    }

    //清空已注册的对象列表，

    //防止重复调用

    $\_PEAR\_destructor\_object\_list = array\(\);

    }

}

....	

register\_shutdown\_function\("\_PEAR\_call\_destructors"\);

&lt;/code&gt;

上面这段代码展示了PEAR是如何实现析构函数的,在构件函数中，将检查当前类中是否有析构函数，如果有，那么将把当前类的引用放入一个全局列表中，在\_PEAR\_call\_destructors中，则检查这个全局列表中的每个元素是否存在相应的析构函数，如果有，则调用它，最后将全局列表清空。

在PEAR.php的最后一行代码，则调用register\_shutdown\_function\("\_PEAR\_call\_destructors"\)，注册\_PEAR\_call\_destructors，这样，当脚本执行完毕的时候，PHP会回调这个函数。使用析构函数，你可以在处理完用户的请求，退出之前做一些必要的"善后"工作，典型的例子是，你可以关闭打开的文件，断开数据库的连接，将某些数据存入磁盘等等。

错误处理

PEAR中可以让你有很多的方式来处理错误，你不仅仅是简单地返回一个错误代码，或者错误的信息，而是可以返回一个PEAR\_Error对象，或者是由PEAR\_Error派生出来的新的错误对象。

PEAR中的错误对象的并没有限定具体的输出形式，它可以仅仅是捕获错误，不给用户返回太多的信息，也可以是去回调一个特殊错误处理函数，同时，即使输出错误信息，它也强迫你必须要是HTML形式，你可以输出XML，CSV形式，或者是其他你自己定义的形式，你只需要从PEAR\_Error派生一个新的类，然后在适当的时候创建并"抛出"这个新类的对象就可以了。

简单的错误处理：

在PEAR中，最简单的错误处理是"抛出"这个错误，你只要简单地创建并返回一个PEAR\_Error的对象就可以了。下面是一个简单的例子：

&lt;code&gt;

function myconnect\($host = "localhost", $port = 1080\)

{

    $fp = fsockopen\($host, $port, $errno, $errstr\);

    if \(!is\_resource\($fp\)\) {

        return new PEAR\_Error\($errstr, $errno\);

    }

    return $fp;

}

$sock = myconnect\(\);

if \(PEAR::isError\($sock\)\) {

    print "connect error: ".$sock-&gt;getMessage\(\)."&lt;BR&gt;\n"

}

&lt;/code&gt;

如上面代码所展示的，在执行一段可能产生错误的代码后，你需要使用PEAR的isError来检测是否存在错误，并且可以使用PEAR\_Error的getMessage来取得最近一次的错误信息。注意：一定要在关键的地方使用使用PEAR::isError

使用raiseError

PHP4.0.5以后，PEAR多了2个函数：

setErrorHandling\($mode, $options = null\)

raiseError\($message = null, $code = null, $mode = null,$options = null,

  $userinfo = null\)

前者可以设置PEAR缺省的错误处理模式，后者是一个包装函数，返回一个PEAR\_Error的对象，和直接创建并返回PEAR\_Error的对象略有不同的是，如果省略$mode,$options等参数，它会使用缺省值来创建这个PEAR\_Error的对象，这些缺省值你可以使用setErrorHandling\(\)来定制。

PEAR\_Error

PEAR\_Error是PEAR的错误对象的一个基类，和PEAR不同，一般来说，你可以直接创建PEAR\_Error的实例，创建方式： $error = new PEAR\_Error\($message, $code, $mode, $options, $userinfo\);

$message是你的错误信息，$code是该错误的错误号，后3个参数是紧密联系的：

$mode:是这个错误的处理模式，可以下列常量：

PEAR\_ERROR\_RETURN：仅仅返回该错误对象（缺省方式）

PEAR\_ERROR\_PRINT：在构建函数中打印这个错误信息，但是当前程序会继续运行。

PEAR\_ERROR\_TRIGGER：使用PHP的trigger\_error\(\) 触发一个错误，如果你已经设置了错误处理函数，或者你把PHP的错误处理级别设置为E\_USER\_ERROR，那么当前程序将会被终止。

PEAR\_ERROR\_DIE：打印错误并退出，程序终止。

PEAR\_ERROR\_CALLBACK：使用一个回调函数或者方法来处理当前错误，程序终止。

$options：这个参数只有在$mode是PEAR\_ERROR\_TRIGGER和PEAR\_ERROR\_CALLBACK的时候才起作用，如果是PEAR\_ERROR\_TRIGGER，$options必须是E\_USER\_NOTICE, E\_USER\_WARNING 或 E\_USER\_ERROR这3个常量的一个，同PHP中trigger\_error的值一致。如果$mode是PEAR\_ERROR\_CALLBACK，$options可以是一个字符串，内容是要回调的函数名，也可以是一个2元素的数组，分别是一个对象变量，和一个字符串（标明要调用的方法）。

$userinfo:存放附加的用户信息，你可以把相关的调试信息放在这里。

PEAR\_Error中有一些常用的方法，这些方法在PHP文挡没有描述，这里一一列出：

int getMode：返回当前的错误处理模式,整型。

string getMessage：返回当前完整的错误信息，字符串。

mixed getCallback：返回当前的回调信息，可能是所回调的函数名，或者是（对象，方法）的数组。

int getCode：返回整型的错误代码。

string getType：返回错误的类型，也就是当前的类名，字符串。

string getUserInfo：返回附加的用户信息，字符串。

string getDebugInfo：内容同上。

string toString：返回当前对象的详细字符串描述，内容包括错误处理的模式，级别，错误信息，错误代码，相关回调函数等等。

回页首

总结

至此，对于PEAR的介绍就结束了。概括地说，如果你要做一个PEAR的扩展应用，需要这么做：

require\_once "PEAR.php"

使用class your\_pear\_extend extends PEAR{}定义你的新类。

在你的类的构造函数中，调用父类PEAR的构造函数：

function your\_pear\_extend{

    $this-&gt;PEAR\(\);

    ...

}

如果需要，定义你的析构函数 \_your\_pear\_extend

如果需要，从PEAR\_Error派生出你自己的错误处理类

设置你的错误处理模式，并在适当的时候触发错误。

在执行可能产生错误的代码后，用PEAR::isError\($obj\)捕获相应的错误。

实现你自己的功能。

