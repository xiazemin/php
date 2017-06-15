# 使用vld

$ vi test.php

$a='123';

echo $a;

$ php -dvld.active=1 ./test.php

Finding entry points

Branch analysis from position: 0

Jump found. \(Code = 62\) Position 1 = -2

filename:       /Users/didi/PHP/test.php

function name:  \(null\)

number of ops:  3

compiled vars:  !0 = $a

line     \#\* E I O op                           fetch          ext  return  operands

---

2     0  E &gt;   ASSIGN                                                   !0, '123'

3     1        ECHO                                                     !0

4     2      &gt; RETURN                                                   1

branch: \#  0; line:     2-    4; sop:     0; eop:     2; out1:  -2

path \#1: 0,

如上为VLD输出的PHP代码生成的中间代码的信息，说明如下：

Branch analysis from position 这条信息多在分析数组时使用。

Return found 是否返回，这个基本上有都有。

filename 分析的文件名

function name 函数名，针对每个函数VLD都会生成一段如上的独立的信息，这里显示当前函数的名称

number of ops 生成的操作数

compiled vars 编译期间的变量，这些变量是在PHP5后添加的，它是一个缓存优化。这样的变量在PHP源码中以IS\_CV标记。

op list 生成的中间代码的变量列表

使用-dvld.active参数输出的是VLD默认设置，如果想看更加详细的内容。可以使用-dvld.verbosity参数。

 

\#php -dvld.active=1 -dvld.verbosity=3 text.php

-dvld.verbosity=3是VLD在当前版本可以显示的最详细的信息.

如果我们只是想要看输出的中间代码，并不想执行这段PHP代码，可以使用-dvld.execute=0来禁用代码的执行

\#php -dvld.active=1 -dvld.execute=0 text.php

 

VLD扩展的参数列表：

-dvld.active 是否在执行PHP时激活VLD挂钩，默认为0，表示禁用。可以使用-dvld.active=1启用。

-dvld.skip\_prepend 是否跳过php.ini配置文件中auto\_prepend\_file指定的文件， 默认为0，即不跳过包含的文件，显示这些包含的文件中的代码所生成的中间代码。此参数生效有一个前提条件：-dvld.execute=0

-dvld.skip\_append 是否跳过php.ini配置文件中auto\_append\_file指定的文件， 默认为0，即不跳过包含的文件，显示这些包含的文件中的代码所生成的中间代码。此参数生效有一个前提条件：-dvld.execute=0

-dvld.execute 是否执行这段PHP脚本，默认值为1，表示执行。可以使用-dvld.execute=0，表示只显示中间代码，不执行生成的中间代码。

-dvld.format 是否以自定义的格式显示，默认为0，表示否。可以使用-dvld.format=1，表示以自己定义的格式显示。这里自定义的格式输出是以-dvld.col\_sep指定的参数间隔

-dvld.col\_sep 在-dvld.format参数启用时此函数才会有效，默认为 “\t”。

-dvld.verbosity 是否显示更详细的信息，默认为1，其值可以为0,1,2,3 其实比0小的也可以，只是效果和0一样，比如0.1之类，但是负数除外，负数和效果和3的效果一样 比3大的值也是可以的，只是效果和3一样。

-dvld.save\_dir 指定文件输出的路径，默认路径为/tmp。

-dvld.save\_paths 控制是否输出文件，默认为0，表示不输出文件

-dvld.dump\_paths 控制输出的内容，现在只有0和1两种情况，默认为1,输出内容

