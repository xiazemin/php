# yacc

yacc文件和lex文件格式一样

标记习惯上大写，其他小写

每条规则都由：左侧名字，右侧符号列表和动作代码以及指示规则结束的；组成

$ yacc ch1-05.y

$ ls

ch1-02.l    ch1-05.y    verb.exe     ch1-04.l    lex.yy.c    y.tab.c

语法分析程序需要来自输入的标记时，调用词法分析程序yylex（），词法分析程序找到识别的词后就返回语法分析程序

可以把y.tab.h文件包含在词法分析程序中，词法分析动作段采用这些定义的预处理符号

$ yacc -d ch1-06.y

$ cc -c lex.yy.c y.tab.c

ch1-05.l:21:8: error: use of undeclared identifier 'PRONOUN'; did

      you mean 'PRNOUN'?

{state=PRONOUN;}

$ cc -c lex.yy.c y.tab.c

ch1-05.l:26:1: warning: implicit declaration of function 'addWord'

      is invalid in C99 \[-Wimplicit-function-declaration\]

addWord\(state,yytext\);

^

ch1-05.l:28:8: warning: implicit declaration of function

      'lookupWord' is invalid in C99

      \[-Wimplicit-function-declaration\]

switch\(lookupWord\(yytext\)\){

       ^

2 warnings generated.

