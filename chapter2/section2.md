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

-------------------------------------------------------------------------------------

   2     0  E &gt;   ASSIGN                                                   !0, '123'

   3     1        ECHO                                                     !0

   4     2      &gt; RETURN                                                   1



branch: \#  0; line:     2-    4; sop:     0; eop:     2; out1:  -2

path \#1: 0,

