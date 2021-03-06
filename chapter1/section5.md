#  Lex 快速入门

Lex 与 Yacc 介绍

Lex 和 Yacc 是 UNIX 两个非常重要的、功能强大的工具。事实上，如果你熟练掌握 Lex 和 Yacc 的话，它们的强大功能使创建 FORTRAN 和 C 的编译器如同儿戏。Ashish Bansal 为您详细的讨论了编写自己的语言和编译器所用到的这两种工具，包括常规表达式、声明、匹配模式、变量、Yacc 语法和解析器代码。最后，他解释了怎样把 Lex 和 Yacc 结合起来。

5 评论

Ashish Bansal \(abansal@ieee.org\), 软件工程师, Sapient 公司

2000 年 11 月 01 日

expand

内容

在 IBM Bluemix 云平台上开发并部署您的下一个应用。

开始您的试用

Lex 代表 Lexical Analyzar。Yacc 代表 Yet Another Compiler Compiler。 让我们从 Lex 开始吧。

Lex

Lex 是一种生成扫描器的工具。扫描器是一种识别文本中的词汇模式的程序。 这些词汇模式（或者常规表达式）在一种特殊的句子结构中定义，这个我们一会儿就要讨论。

一种匹配的常规表达式可能会包含相关的动作。这一动作可能还包括返回一个标记。 当 Lex 接收到文件或文本形式的输入时，它试图将文本与常规表达式进行匹配。 它一次读入一个输入字符，直到找到一个匹配的模式。 如果能够找到一个匹配的模式，Lex 就执行相关的动作（可能包括返回一个标记）。 另一方面，如果没有可以匹配的常规表达式，将会停止进一步的处理，Lex 将显示一个错误消息。

Lex 和 C 是强耦合的。一个 .lex 文件（Lex 文件具有 .lex 的扩展名）通过 lex 公用程序来传递，并生成 C 的输出文件。这些文件被编译为词法分析器的可执行版本。

回页首

Lex 的常规表达式

常规表达式是一种使用元语言的模式描述。表达式由符号组成。符号一般是字符和数字，但是 Lex 中还有一些具有特殊含义的其他标记。 下面两个表格定义了 Lex 中使用的一些标记并给出了几个典型的例子。

用 Lex 定义常规表达式

字符    含义

A-Z, 0-9, a-z    构成了部分模式的字符和数字。

.    匹配任意字符，除了 \n。

* 用来指定范围。例如：A-Z 指从 A 到 Z 之间的所有字符。

\[ \]    一个字符集合。匹配括号内的 任意 字符。如果第一个字符是 ^ 那么它表示否定模式。例如: \[abC\] 匹配 a, b, 和 C中的任何一个。

\*    匹配 0个或者多个上述的模式。

* 匹配 1个或者多个上述模式。

?    匹配 0个或1个上述模式。

$    作为模式的最后一个字符匹配一行的结尾。

{ }    指出一个模式可能出现的次数。 例如: A{1,3} 表示 A 可能出现1次或3次。

```
用来转义元字符。同样用来覆盖字符在此表中定义的特殊意义，只取字符的本意。
```

^    否定。

\|    表达式间的逻辑或。

"&lt;一些符号&gt;"    字符的字面含义。元字符具有。

/    向前匹配。如果在匹配的模版中的“/”后跟有后续表达式，只匹配模版中“/”前 面的部分。如：如果输入 A01，那么在模版 A0/1 中的 A0 是匹配的。

\( \)    将一系列常规表达式分组。

常规表达式举例

常规表达式    含义

joke\[rs\]    匹配 jokes 或 joker。

A{1,2}shis+    匹配 AAshis, Ashis, AAshi, Ashi。

\(A\[b-e\]\)+    匹配在 A 出现位置后跟随的从 b 到 e 的所有字符中的 0 个或 1个。

Lex 中的标记声明类似 C 中的变量名。每个标记都有一个相关的表达式。 （下表中给出了标记和表达式的例子。） 使用这个表中的例子，我们就可以编一个字数统计的程序了。 我们的第一个任务就是说明如何声明标记。

标记声明举例

标记    相关表达式    含义

数字\(number\)    \(\[0-9\]\)+    1个或多个数字

字符\(chars\)    \[A-Za-z\]    任意字符

空格\(blank\)    " "    一个空格

字\(word\)    \(chars\)+    1个或多个 chars

变量\(variable\)    \(字符\)+\(数字\)\*\(字符\)\*\(数字\)\*

回页首

Lex 编程

Lex 编程可以分为三步：

以 Lex 可以理解的格式指定模式相关的动作。

在这一文件上运行 Lex，生成扫描器的 C 代码。

编译和链接 C 代码，生成可执行的扫描器。

注意: 如果扫描器是用 Yacc 开发的解析器的一部分，只需要进行第一步和第二步。 关于这一特殊问题的帮助请阅读 Yacc和 将 Lex 和 Yacc 结合起来部分。

现在让我们来看一看 Lex 可以理解的程序格式。一个 Lex 程序分为三个段：第一段是 C 和 Lex 的全局声明，第二段包括模式（C 代码），第三段是补充的 C 函数。 例如, 第三段中一般都有 main\(\) 函数。这些段以%%来分界。 那么，回到字数统计的 Lex 程序，让我们看一下程序不同段的构成。

回页首

C 和 Lex 的全局声明

这一段中我们可以增加 C 变量声明。这里我们将为字数统计程序声明一个整型变量，来保存程序统计出来的字数。 我们还将进行 Lex 的标记声明。

字数统计程序的声明

```
   %{

    int wordCount = 0;

    %}

    chars \[A-za-z\\_\'\.\"\]

    numbers \(\[0-9\]\)+

    delim \[" "\n\t\]

    whitespace {delim}+

    words {chars}+

    %%
```

两个百分号标记指出了 Lex 程序中这一段的结束和三段中第二段的开始。

回页首

Lex 的模式匹配规则

让我们看一下 Lex 描述我们所要匹配的标记的规则。（我们将使用 C 来定义标记匹配后的动作。） 继续看我们的字数统计程序，下面是标记匹配的规则。

字数统计程序中的 Lex 规则

```
   {words} { wordCount++; /\*

    increase the word count by one\*/ }

    {whitespace} { /\* do

    nothing\*/ }

    {numbers} { /\* one may

    want to add some processing here\*/ }

    %%
```

回页首

C 代码

Lex 编程的第三段，也就是最后一段覆盖了 C 的函数声明（有时是主函数）。注意这一段必须包括 yywrap\(\) 函数。 Lex 有一套可供使用的函数和变量。 其中之一就是 yywrap。 一般来说，yywrap\(\) 的定义如下例。我们将在 高级 Lex 中探讨这一问题。

字数统计程序的 C 代码段

```
   void main\(\)

    {

    yylex\(\); /\* start the

    analysis\*/

    printf\(" No of words:

    %d\n", wordCount\);

    }

    int yywrap\(\)

    {

    return 1;

    }
```

上一节我们讨论了 Lex 编程的基本元素，它将帮助你编写简单的词法分析程序。 在 高级 Lex 这一节中我们将讨论 Lex 提供的函数，这样你就能编写更加复杂的程序了。

回页首

将它们全部结合起来

.lex文件是 Lex 的扫描器。它在 Lex 程序中如下表示：

$ lex &lt;file name.lex&gt;

这生成了 lex.yy.c 文件，它可以用 C 编译器来进行编译。它还可以用解析器来生成可执行程序，或者在链接步骤中通过选项 �ll 包含 Lex 库。

这里是一些 Lex 的标志：

-c表示 C 动作，它是缺省的。

-t写入 lex.yy.c 程序来代替标准输出。

-v提供一个两行的统计汇总。

-n不打印 -v 的汇总。

回页首

高级 Lex

Lex 有几个函数和变量提供了不同的信息，可以用来编译实现复杂函数的程序。 下表中列出了一些变量和函数，以及它们的使用。 详尽的列表请参考 Lex 或 Flex 手册（见后文的 资源）。

Lex 变量

yyin    FILE\* 类型。 它指向 lexer 正在解析的当前文件。

yyout    FILE\* 类型。 它指向记录 lexer 输出的位置。 缺省情况下，yyin 和 yyout 都指向标准输入和输出。

yytext    匹配模式的文本存储在这一变量中（char\*）。

yyleng    给出匹配模式的长度。

yylineno    提供当前的行数信息。 （lexer不一定支持。）

Lex 函数

yylex\(\)    这一函数开始分析。 它由 Lex 自动生成。

yywrap\(\)    这一函数在文件（或输入）的末尾调用。 如果函数的返回值是1，就停止解析。 因此它可以用来解析多个文件。 代码可以写在第三段，这就能够解析多个文件。 方法是使用 yyin 文件指针（见上表）指向不同的文件，直到所有的文件都被解析。 最后，yywrap\(\) 可以返回 1 来表示解析的结束。

yyless\(int n\)    这一函数可以用来送回除了前�n? 个字符外的所有读出标记。

yymore\(\)    这一函数告诉 Lexer 将下一个标记附加到当前标记后。

对 Lex 的讨论就到这里。下面我们来讨论 Yacc...

回页首

