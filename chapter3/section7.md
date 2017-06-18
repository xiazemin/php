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

```
  you mean 'PRNOUN'?
```

{state=PRONOUN;}

$ cc -c lex.yy.c y.tab.c

ch1-05.l:26:1: warning: implicit declaration of function 'addWord'

```
  is invalid in C99 \[-Wimplicit-function-declaration\]
```

addWord\(state,yytext\);

^

ch1-05.l:28:8: warning: implicit declaration of function

```
  'lookupWord' is invalid in C99

  \[-Wimplicit-function-declaration\]
```

switch\(lookupWord\(yytext\)\){

```
   ^
```

2 warnings generated.

函数没有加声明，加上就好了

$ cc -c lex.yy.c y.tab.c

y.tab.c:1231:16: warning: implicit declaration of function 'yylex'

```
  is invalid in C99 \[-Wimplicit-function-declaration\]

  yychar = YYLEX;

           ^
```

y.tab.c:587:16: note: expanded from macro 'YYLEX'

\# define YYLEX yylex \(\)

```
           ^
```

1 warning generated.

$ cc -o sentense.exe lex.yy.o y.tab.o -ll -v

Apple LLVM version 7.3.0 \(clang-703.0.31\)

duplicate symbol \_main in:

    lex.yy.o

    y.tab.o

ld: 1 duplicate symbol for architecture x86\_64

clang: error: linker command failed with exit code 1 \(use -v to see invocation\)

原因：lex中多定义了一个main函数，注视掉即可

$ cc -o sentense.exe lex.yy.o y.tab.o -ll -v

Apple LLVM version 7.3.0 \(clang-703.0.31\)

Target: x86\_64-apple-darwin15.0.0

Thread model: posix

InstalledDir: /Library/Developer/CommandLineTools/usr/bin

 "/Library/Developer/CommandLineTools/usr/bin/ld" -demangle -dynamic -arch x86\_64 -macosx\_version\_min 10.11.0 -o sentense.exe lex.yy.o y.tab.o -ll -lSystem /Library/Developer/CommandLineTools/usr/bin/../lib/clang/7.3.0/lib/darwin/libclang\_rt.osx.a

