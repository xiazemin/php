# lex&yacc

yacc ­d bas.y         \# 生成 y.tab.h, y.tab.c

lex bas.l            \# 生成 lex.yy.c

cc lex.yy.c y.tab.c ­o bas.exe   \# 编译/连接  
Yacc 读入 bas.y 中的语法描述而后生成一个剖析器,即 y.tab.c 中的函数 yyparse。bas.y 中包含  
的是一系列的标记声明。“­d”选项使

Yacc 生成标记声明并且把它们保存在 y.tab.c 中。Lex 读入 bas.l 中的正则表达式的说明,包含文件 y.tab.h,然后生成词汇解释器,即文件 lex.yy.c 中的函数 yylex。  
最终,这个解释器和剖析器被连接到一起,而组成一个可执行程序,bas.exe。我们从 main 函 数中调用 yyparse 来运行这个编译器。函数 yyparse 自动调用 yylex 以便获取每一个标志。

... 定义 ...

%%

... 规则 ...

%%

... 子程序 ...

lex 的输入文件分成三个段,段间用 %% 来分隔。

最短 的可用 lex 文件:

%%

输入字符将被一个字符一个字符直接输出。由于必须存在一个规则段,第一个 %% 总是要求存在 的。然而,如果我们不指定任何规则,默认动作就是匹配任意字符然后直接输出到输出文件。默认 的输入文件和输出文件分别是 stdin 和 stdout。

下面是效果完全相同的例子,显式表达了默认代码: 

%%

/\* 匹配除换行外的任意字符 \*/

 . ECHO;

/\* 匹配换行符 \*/

 \n ECHO;

%%

int yywrap\(void\)

 {return 1; }

int main\(void\) 

{ yylex\(\);

return 0; }

