# lex语法

定义段可以定义规则名称，供规则段使用

规则名  规则

规则段使用规则

｛规则名｝｛动作｝

默认从标准输入读取，如果自定义可以yyin＝文件句柄

如果到达输入文件末尾，可以调用yywarp（），用户定义返回0（结束）或者1（继续）可以用来处理多文件

使用input宏得到字符，使用unput放回



