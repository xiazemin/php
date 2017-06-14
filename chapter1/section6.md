# Yacc  快速入门

Yacc

Yacc 代表 Yet Another Compiler Compiler。 Yacc 的 GNU 版叫做 Bison。它是一种工具，将任何一种编程语言的所有语法翻译成针对此种语言的 Yacc 语 法解析器。它用巴科斯范式\(BNF, Backus Naur Form\)来书写。按照惯例，Yacc 文件有 .y 后缀。编译行如下调用 Yacc 编译器：

$ yacc &lt;options&gt;

```
&lt;filename ending with .y&gt;
```

在进一步阐述以前，让我们复习一下什么是语法。在上一节中，我们看到 Lex 从输入序列中识别标记。 如果你在查看标记序列，你可能想在这一序列出现时执行某一动作。 这种情况下有效序列的规范称为语法。Yacc 语法文件包括这一语法规范。 它还包含了序列匹配时你想要做的事。

为了更加说清这一概念，让我们以英语为例。 这一套标记可能是：名词, 动词, 形容词等等。为了使用这些标记造一个语法正确的句子，你的结构必须符合一定的规则。 一个简单的句子可能是名词+动词或者名词+动词+名词。\(如 I care. See spot run.\)

所以在我们这里，标记本身来自语言（Lex），并且标记序列允许用 Yacc 来指定这些标记\(标记序列也叫语法\)。

终端和非终端符号

终端符号 : 代表一类在语法结构上等效的标记。 终端符号有三种类型：

命名标记: 这些由 %token 标识符来定义。 按照惯例，它们都是大写。

字符标记 : 字符常量的写法与 C 相同。例如, -- 就是一个字符标记。

字符串标记 : 写法与 C 的字符串常量相同。例如，"&lt;&lt;" 就是一个字符串标记。

lexer 返回命名标记。

非终端符号 : 是一组非终端符号和终端符号组成的符号。 按照惯例，它们都是小写。 在例子中，file 是一个非终端标记而 NAME 是一个终端标记。

用 Yacc 来创建一个编译器包括四个步骤：

通过在语法文件上运行 Yacc 生成一个解析器。

说明语法：

编写一个 .y 的语法文件（同时说明 C 在这里要进行的动作）。

编写一个词法分析器来处理输入并将标记传递给解析器。 这可以使用 Lex 来完成。

编写一个函数，通过调用 yyparse\(\) 来开始解析。

编写错误处理例程（如 yyerror\(\)）。

编译 Yacc 生成的代码以及其他相关的源文件。

将目标文件链接到适当的可执行解析器库。

回页首

用 Yacc 编写语法

如同 Lex 一样, 一个 Yacc 程序也用双百分号分为三段。 它们是：声明、语法规则和 C 代码。 我们将解析一个格式为 姓名 = 年龄 的文件作为例子，来说明语法规则。 我们假设文件有多个姓名和年龄，它们以空格分隔。 在看 Yacc 程序的每一段时，我们将为我们的例子编写一个语法文件。

回页首

C 与 Yacc 的声明

C 声明可能会定义动作中使用的类型和变量，以及宏。 还可以包含头文件。每个 Yacc 声明段声明了终端符号和非终端符号（标记）的名称，还可能描述操作符优先级和针对不同符号的数据类型。 lexer \(Lex\) 一般返回这些标记。所有这些标记都必须在 Yacc 声明中进行说明。

在文件解析的例子中我们感兴趣的是这些标记：name, equal sign, 和 age。Name 是一个完全由字符组成的值。 Age 是数字。于是声明段就会像这样：

文件解析例子的声明

%

```
\\#typedef char\\* string; /\\*



to specify token types as char\\* \\*/



\\#define YYSTYPE string /\\*



a Yacc variable which has the value of returned token \\*/



%}



%token NAME EQ AGE



%%
```

你可能会觉得 YYSTYPE 有点奇怪。但是类似 Lex, Yacc 也有一套变量和函数可供用户来进行功能扩展。 YYSTYPE 定义了用来将值从 lexer 拷贝到解析器或者 Yacc 的 yylval （另一个 Yacc 变量）的类型。 默认的类型是 int。 由于字符串可以从 lexer 拷贝，类型被重定义为 char\*。 关于 Yacc 变量的详细讨论，请参考 Yacc 手册（见 资源）。

回页首

Yacc 语法规则

Yacc 语法规则具有以下一般格式：

result: components { /\\*

```
action to be taken in C \\*/ }



;
```

在这个例子中，result 是规则描述的非终端符号。Components 是根据规则放在一起的不同的终端和非终端符号。 如果匹配特定序列的话 Components 后面可以跟随要执行的动作。 考虑如下的例子：

param : NAME EQ NAME {

```
printf\\("\tName:%s\tValue\\(name\\):%s\n", $1,$3\\);}



    \\| NAME EQ VALUE{



    printf\\("\tName:%s\tValue\\(value\\):%s\n",$1,$3\\);}



;
```

如果上例中序列 NAME EQ NAME 被匹配，将执行相应的 { } 括号中的动作。 这里另一个有用的就是 $1 和 $3 的使用, 它们引用了标记 NAME 和 NAME（或者第二行的 VALUE）的值。 lexer 通过 Yacc 的变量 yylval 返回这些值。标记 NAME 的 Lex 代码是这样的：

char \\[A-Za-z\\]

```
name {char}+



%%



{name} { yylval = strdup\\(yytext\\);



return NAME; }
```

文件解析例子的规则段是这样的：

文件解析的语法

file : record file

```
\\| record



;



record: NAME EQ AGE {



printf\\("%s is now %s years old!!!", $1, $3\\);}



;



%%
```

回页首

附加 C 代码

现在让我们看一下语法文件的最后一段，附加 C 代码。 （这一段是可选的，如果有人想要略过它的话：）一个函数如 main\(\) 调用 yyparse\(\) 函数（Yacc 中 Lex 的 yylex\(\) 等效函数）。 一般来说，Yacc 最好提供 yyerror\(char msg\) 函数的代码。 当解析器遇到错误时调用 yyerror\(char msg\)。错误消息作为参数来传递。 一个简单的 yyerror\( char\* \) 可能是这样的：

int yyerror\\(char\\* msg\\)

```
{



printf\\("Error: %s



encountered at line number:%d\n", msg, yylineno\\);



}
```

yylineno 提供了行数信息。

这一段还包括文件解析例子的主函数：

附加 C 代码

void main\\(\\)

```
{



    yyparse\\(\\);



}



int yyerror\\(char\\* msg\\)



{



printf\\("Error: %s



encountered \n", msg\\);
```

要生成代码，可能用到以下命令：

$ yacc \\_d &lt;filename.y&gt;

这生成了输出文件 y.tab.h 和 y.tab.c，它们可以用 UNIX 上的任何标准 C 编译器来编译（如 gcc）。

回页首

命令行的其他常用选项

'-d' ,'--defines' : 编写额外的输出文件，它们包含这些宏定义：语法中定义的标记类型名称，语义的取值类型 YYSTYPE, 以及一些外部变量声明。如果解析器输出文件名叫 'name.c', 那么 '-d' 文件就叫做 'name.h'。 如果你想将 yylex 定义放到独立的源文件中，你需要 'name.h', 因为 yylex 必须能够引用标记类型代码和 yylval变量。

'-b file-prefix' ,'--file-prefix=prefix' : 指定一个所有Yacc输出文件名都可以使用的前缀。选择一个名字，就如输入文件名叫 'prefix.c'.

'-o outfile' ,'--output-file=outfile' : 指定解析器文件的输出文件名。其他输出文件根据 '-d' 选项描述的输出文件来命名。

Yacc 库通常在编译步骤中自动被包括。但是它也能被显式的包括，以便在编译步骤中指定 �ly选项。这种情况下的编译命令行是：

$ cc &lt;source file

```
names&gt; -ly
```

回页首

