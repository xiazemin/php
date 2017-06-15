# 在 php 中使用 strace、gdb、tcpdump 调试工具

在 PHP 中我们最常使用调试方式是输出打印方式，比如通过 echo、var\_dump 输出信息到终端或者通过 fwrite、file\_put\_contents 将信息写入到文件中。这种普通方式能帮我们解决绝大部分调试问题。但仍然有些问题是需要借助其他工具来分析的，比如死循环，程序执行时间超预期，占用 cpu 过高，php 内核或者扩展错误等场景，这时如果借助 strace、gdb、tcpdump 这样的工具就能很好的去帮助我们定位问题。



strace



strace 是 Linux 环境下的一款程序调试工具，用来监察一个应用程序所使用的系统调用及它所接收的系统信息。



在 linux 中，进程是不能直接去访问硬件设备\(比如读取磁盘文件，接收网络数据等等\)，但可以将用户态模式切换至内核态模式，通过系统调用来访问硬件设备。这时 strace 就可以跟踪到一个进程产生的系统调用，包括参数，返回值，执行消耗的时间，调用次数，成功和失败的次数。



比如我们使用 strace 来跟踪 cat 查看一个文件做了什么：



\[root@syyong home\]$ strace cat index.php

execve\("/bin/cat", \["cat", "index.php"\], \[/\* 25 vars \*/\]\) = 0

brk\(0\)                                  = 0x21b0000

mmap\(NULL, 4096, PROT\_READ\|PROT\_WRITE, MAP\_PRIVATE\|MAP\_ANONYMOUS, -1, 0\) = 0x7f5fd02fd000

access\("/etc/ld.so.preload", R\_OK\)      = -1 ENOENT \(No such file or directory\)

open\("/etc/ld.so.cache", O\_RDONLY\)      = 3

fstat\(3, {st\_mode=S\_IFREG\|0644, st\_size=41783, ...}\) = 0

mmap\(NULL, 41783, PROT\_READ, MAP\_PRIVATE, 3, 0\) = 0x7f5fd02f2000

close\(3\)                                = 0

open\("/lib64/libc.so.6", O\_RDONLY\)      = 3

...                               

fstat\(1, {st\_mode=S\_IFCHR\|0620, st\_rdev=makedev\(136, 3\), ...}\) = 0

open\("index.php", O\_RDONLY\)             = 3

fstat\(3, {st\_mode=S\_IFREG\|0664, st\_size=27, ...}\) = 0

read\(3, "&lt;?php\necho 'hello world';\n\n", 32768\) = 27

write\(1, "&lt;?php\necho 'hello world';\n\n", 27&lt;?php

echo 'hello world';



\) = 27

read\(3, "", 32768\)                      = 0

close\(3\)                                = 0

close\(1\)                                = 0

close\(2\)                                = 0

exit\_group\(0\)                           = ?

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

\[root@syyong home\]$ strace -e read cat index.php

read\(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0&gt;\0\1\0\0\0p\356\1\0\0\0\0\0"..., 832\) = 832

read\(3, "&lt;?php\necho 'hello world';\n\n", 32768\) = 27

&lt;?php

echo 'hello world';



read\(3, "", 32768\)                      = 0

+++ exited with 0 +++

1

2

3

4

5

6

7

8

1

2

3

4

5

6

7

8

\[root@syyong home\]$ strace -c cat index.php

&lt;?php

echo "hello world";



% time     seconds  usecs/call     calls    errors syscall

------ ----------- ----------- --------- --------- ----------------

  0.00    0.000000           0         3           read

  0.00    0.000000           0         1           write

  0.00    0.000000           0         4           open

  0.00    0.000000           0         6           close

  0.00    0.000000           0         5           fstat

  0.00    0.000000           0         9           mmap

  0.00    0.000000           0         3           mprotect

  0.00    0.000000           0         1           munmap

  0.00    0.000000           0         3           brk

  0.00    0.000000           0         1         1 access

  0.00    0.000000           0         1           execve

  0.00    0.000000           0         1           arch\_prctl

------ ----------- ----------- --------- --------- ----------------

100.00    0.000000                    38         1 total

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

\[root@syyong home\]$ strace -T cat index.php 2&gt;&1\|grep read      

read\(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0&gt;\0\1\0\0\0p\356\1\0\0\0\0\0"..., 832\) = 832 &lt;0.000015&gt;

read\(3, "&lt;?php\necho 'hello world';\n\n", 32768\) = 27 &lt;0.000019&gt;

read\(3, "", 32768\)                      = 0 &lt;0.000014&gt;

1

2

3

4

1

2

3

4

默认返回的结果每一行代表一条系统调用，规则为“系统调用的函数名及其参数=函数返回值”。也可以外加一些条件比如：-e 指定返回的调用函数，-c 对结果进行统计，-T 查看绝对耗时，-p 通过 pid 附着\(attach\)到任何运行的进程等等。



strace 的使用方法这里就不做具体介绍了，可以通过 strace –help 去详细了解使用方法。



那么通过 strace 拿到了所有程序去调用系统过程所产生的痕迹后，我们能用来定位哪些问题呢？



调试性能问题，查看系统调用的频率，找出耗时的程序段

查看程序读取的是哪些文件从而定位比如配置文件加载错误问题

查看某个 php 脚本长时间运行“假死”情况

当程序出现 “Out of memory” 时被系统发出的 SIGKILL 信息所 kill

另外因为 strace 拿到的是系统调用相关信息，一般也即是 IO 操作信息，这个对于排查比如 cpu 占用100%问题是无能为力的。这个时候就可以使用 GDB 工具了。



phptrace 

因为 strace 只能追踪到系统调用信息，而拿不到php代码层的调用信息。phptrace扩展就是为了解决这个问题，phptrace 包含两个功能：1. 打印当前 PHP 调用栈，2. 实时追踪 PHP 调用。这样就能更方便我们去查看到我们需要的信息。



GDB



gdb 是一个由 GNU 开源组织发布运行在 UNIX/LINUX 操作系统下功能强大的程序调试工具。使用 gdb 可以在程序运行时观察程序的内部结构和内存的使用情况，当程序 dump 时还可以通过 gdb 观察到程序 dump 前发生了什么。主要来说 gdb 具有以下2个功能：



跟踪和变更执行计算机程序

断点功能

因为 php 语言是 c 写的，那么使用 gdb 也就能很方便的去调试 php 代码。举例，我们通过 gdb 来调试一个简单的 php 程序 index.php：



// 程序代码：

&lt;?php

for \($i = 0; $i &lt; 3; $i ++\) {

    echo $i . PHP\_EOL;

    if \($i == 2\) {

        $j = $i + 1;

        var\_dump\($j\);

    }

    sleep\(1\);

}

1

2

3

4

5

6

7

8

9

10

1

2

3

4

5

6

7

8

9

10

gdb 开始调试：



\[root@syyong home\]$ sudo gdb php

\(gdb\)run index.php

...

0

1

2

int\(3\)

\[Inferior 1 \(process 577\) exited normally\]

1

2

3

4

5

6

7

8

1

2

3

4

5

6

7

8

注：如果 mac 下使用 gdb 时报：“…please check gdb is codesigned - see taskgated\(8\)…”时可参考https://leandre.cn/search/gdb/➫。gdb 在调试程序时，如果 ulimit 打开则会把错误信息打印到当前目录下的 core.\* 文件中。ulimit -c 如果为0则表示没打开，可以执行 ulimit -c unlimited 或者 ulimit -c 大于0的数字。



常用命令：



p：print，打印 C 变量的值

c：continue，继续运行被中止的程序

b：breakpoint，设置断点，可以按照函数名设置，如 b zif\_php\_function，也可以按照源代码的行数指定断点，如 b src/networker/Server.c:1000

t：thread，切换线程，如果进程拥有多个线程，可以使用 t 指令，切换到不同的线程

ctrl + c：中断当前正在运行的程序，和 c 指令配合使用

n：next，执行下一行，单步调试

info threads：查看运行的所有线程

l：list，查看源码，可以使用 l 函数名 或者 l 行号

bt：backtrace，查看运行时的函数调用栈。当程序出错后用于查看调用栈信息

finish：完成当前函数

f：frame，与 bt 配合使用，可以切换到函数调用栈的某一层

r：run，运行程序

使用 .gdbinit 脚本： 

除了在 gdb shell 里输入命令，也可以预先编写好脚本让 gdb 执行。当 gdb 启动的时候会在当前目录下查找 “.gdbinit” 文件并加载，作为 gdb 命令进行执行。这样就可以不用在命令行中做一些重复的事，比如设定多个断点等操作。另外在 gdb 运行时也可以通过执行“\(gdb\) source \[-s\] \[-v\] filename” 来解释 gdb 命令脚本文件。一个 .gdbinit 文件例子：



file index.php

set args hello

b main

b foo

r

1

2

3

4

5

1

2

3

4

5

php 源码中提供的一个 .gdbinit 示例➫



其他 gdb 常用命令可以参考：



http://linuxtools-rst.readthedocs.io/zh\_CN/latest/tool/gdb.html➫

http://coolshell.cn/articles/3643.html➫

http://www.cnblogs.com/xuqiang/archive/2011/05/02/2034583.html

http://blog.csdn.net/21cnbao/article/details/7385161➫

https://sourceware.org/gdb/current/onlinedocs/gdb/➫

所有 gdb 命令索引➫

gdb 调试 php：



gdb 有3种使用方式：



跟踪正在运行的 PHP 程序，使用 “gdb -p 进程ID” 进行附加到进程上

运行并调试 PHP 程序，使用 “gdb php -&gt; run server.php” 进行调试

当 PHP 程序发生 coredump 后使用 gdb 加载 core 内存镜像进行调试 gdb php core

php 在解释执行过程中，zend 引擎用 executor\_globals 变量保存了执行过程中的各种数据，包括执行函数、文件、代码行等。zend 虚拟机是使用 C 编写，gdb 来打印 PHP 的调用栈时，实际是打印的虚拟机的执行信息。

使用 zbacktrace 更简单的调试： 

php 源代码中还提供了 zbacktrace 这样的方便的对 gdb 命令的封装的工具。zbacktrace 是 PHP 源码包提供的一个 gdb 自定义指令，功能与 bt 指令类似，与bt不同的是 zbacktrace 看到的调用栈是 PHP 函数调用栈，而不是 c 函数。zbacktrace 可以直接看到当前执行函数、文件名和行数，简化了直接使用 gdb 命令的很多步骤。在 php-src➫的根目录中有一个 .gdbinit 文件，下载后再 gdb shell 中输入：



\(gdb\) source .gdbinit

\(gdb\) zbacktrace

1

2

1

2

基于 gdb 的功能特点，我们可以使用gdb来排查比如这些问题：



某个 php 进程占用 cpu 100% 问题

出现 core dump 问题，比如“Segmentation fault”

php 扩展出现错误

死循环问题

一些使用 gdb 排查问题例子：



更简单的重现 PHP Core 的调用栈➫

PHP stream未能及时清理现场导致 Core 的 bug➫

一个低概率的 PHP Core dump➫

tcpdump



即 dump the traffic on a network，是一个功能强大的，命令行的包分析器。它可以将网络中传送的数据包完全截获下来提供分析。它支持针对网络层、协议、主机、网络或端口的过滤，并提供 and、or、not 等逻辑语句来帮助去掉无用的信息。这样我们就能详细看到网络通信的过程，能帮助我们解决很多网络问题。比如可以通过 tcpdump 知道什么时候发起的3次握手什么时候发送 FIN 包，什么时候发送 RST 包。



命令格式为：

tcpdump \[-aAbdDefhHIJKlLnNOpqRStuUvxX\#\] \[ -B size \] \[ -c count \]

           \[ -C file\_size \] \[ -E algo:secret \] \[ -F file \] \[ -G seconds \]

           \[ -i interface \] \[ -j tstamptype \] \[ -M secret \]

           \[ -Q metadata-filter-expression \]

           \[ -r file \] \[ -s snaplen \] \[ -T type \] \[ --version \] \[ -V file \]

           \[ -w file \] \[ -W filecount \] \[ -y datalinktype \] \[ -z command \]

           \[ -Z user \] \[ 表达式 \]

原文链接：在 php 中使用 strace、gdb、tcpdump 调试工具

