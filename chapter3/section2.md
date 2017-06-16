# lex&yacc

yacc ­d bas.y         \# 生成 y.tab.h, y.tab.c

lex bas.l            \# 生成 lex.yy.c

cc lex.yy.c y.tab.c ­o bas.exe   \# 编译/连接Yacc 读入 bas.y 中的语法描述而后生成一个剖析器,即 y.tab.c 中的函数 yyparse。bas.y 中包含的是一系列的标记声明。“­d”选项使

 Yacc 生成标记声明并且把它们保存在 y.tab.c 中。Lex 读入 bas.l 中的正则表达式的说明,包含文件 y.tab.h,然后生成词汇解释器,即文件 lex.yy.c 中的函数 yylex。最终,这个解释器和剖析器被连接到一起,而组成一个可执行程序,bas.exe。我们从 main 函 数中调用 yyparse 来运行这个编译器。函数 yyparse 自动调用 yylex 以便获取每一个标志。

