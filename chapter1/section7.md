# 将Lex 与 Yacc 结合起来

将Lex 与 Yacc 结合起来

到目前为止我们已经分别讨论了 Lex 和 Yacc。现在让我们来看一下他们是怎样结合使用的。

一个程序通常在每次返回一个标记时都要调用 yylex\(\) 函数。只有在文件结束或者出现错误标记时才会终止。

一个由 Yacc 生成的解析器调用 yylex\(\) 函数来获得标记。 yylex\(\) 可以由 Lex 来生成或完全由自己来编写。 对于由 Lex 生成的 lexer 来说，要和 Yacc 结合使用，每当 Lex 中匹配一个模式时都必须返回一个标记。 因此 Lex 中匹配模式时的动作一般格式为：

{pattern} { /\_ do smthg\_/

return TOKEN\\_NAME; }

于是 Yacc 就会获得返回的标记。当 Yacc 编译一个带有 \_d 标记的 .y文件时，会生成一个头文件，它对每个标记都有 \#define 的定义。 如果 Lex 和 Yacc 一起使用的话，头文件必须在相应的 Lex 文件 .lex中的 C 声明段中包括。

让我们回到名字和年龄的文件解析例子中，看一看 Lex 和 Yacc 文件的代码。

Name.y - 语法文件

%

typedef char\\* string;

\\#define YYSTYPE string

%}

%token NAME EQ AGE

%%

file : record file

\\| record

;

record : NAME EQ AGE {

printf\\("%s is %s years old!!!\n", $1, $3\\); }

;

%%

int main\\(\\)

{

yyparse\\(\\);

return 0;

}

int yyerror\\(char \\*msg\\)

{

printf\\("Error

encountered: %s \n", msg\\);

}

Name.lex - Lex 的解析器文件

%{

\\#include "y.tab.h"

\\#include &lt;stdio.h&gt;

\\#include &lt;string.h&gt;

extern char\\* yylval;

%}

char \\[A-Za-z\\]

num \\[0-9\\]

eq \\[=\\]

name {char}+

age {num}+

%%

{name} { yylval = strdup\\(yytext\\);

return NAME; }

{eq} { return EQ; }

{age} { yylval = strdup\\(yytext\\);

return AGE; }

%%

int yywrap\\(\\)

{

return 1;

}

作为一个参考，我们列出了 y.tab.h, Yacc 生成的头文件。

y.tab.h - Yacc 生成的头文件

\\# define NAME 257

\\# define EQ 258

\\# define AGE 259

我们对于 Lex 和 Yacc的讨论到此为止。今天你想要编译什么语言呢？

参考资料

您可以参阅本文在 developerWorks 全球站点上的 英文原文.

Lex and Yacc, Levine, Mason 和 Branson, O�Reilly 及其合作公司，2nd Ed。

Program Development in UNIX, J. T. Shen, Prentice-Hall India。

Compilers: Principles, Techniques and Tools, Ahoo, Sethi 和 Ullman, Addison-Wesley Pub. Co., 1985，11。

Lex and Yacc and compiler writing指导。

Java 版的 Lex 指导, 叫做 Jlex。

使用 Lex 和 Yacc 的 formalizing a grammar实例。

