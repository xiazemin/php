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

｜是一个特殊的动作（与下一个模式相同的动作）

yytext是模式识别出来的文本

1，lex只匹配字符或字符串一次

2，lex执行最长可能匹配操作

ECHO输出匹配的模式

用户例程段

任意合法的c代码，lex将它复制到c文件

产生的c例程名字叫yylex（），生成文件名叫yy.lex.c



