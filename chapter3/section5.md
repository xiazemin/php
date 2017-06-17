# section5

$ lex ch1-02.l

$ gcc lex.yy.c -o verb.exe

Undefined symbols for architecture x86\_64:

"\_yywrap", referenced from:

```
  \_yylex in lex-e33b10.o
```

ld: symbol\(s\) not found for architecture x86\_64

clang: error: linker command failed with exit code 1 \(use -v to see invocation\)

可以在lex.c加入如下的行来解决问题。

\#define yywrap\(\)  1

更好的办法是定义:

int yywrap\(\)

{

return\(1\);

}

$ ls

ch1-02.l    lex.yy.c    verb.exe

定义段

lex将%{%}之间的代码直接拷贝到生成的c代码中

lex用空白缩进表示注释

规则段

每条规则由模式（unix样式正则表达式）和动作组成，中间空格分开



