# PHPT

php使用PHP-QA的.phpt测试系统做单元测试



摘要



测试文件标志大小写敏感，要求是大写。测试标志有三部分：–TEST–，–FILE–，–EXPECT–。



wamp安装pear



见php安装pear



测试文件编写



原文件 str\_replace.PHP



&lt;?php

/\*\*

 \* 使用PHP-QA的.phpt做单元测试

 \*/

$str = 'Hello, all!';

var\_dump\(str\_replace\('all', 'world', $str\)\);

测试文件 str\_replace.phpt



--TEST--

str\_replace\(\) function

--FILE--

&lt;?php

/\*\*

 \* 使用PHP-QA的.phpt做单元测试

 \*/



$str = 'Hello, all!';

echo\(str\_replace\('all', 'world', $str\)\);

?&gt;

--EXPECT--

Hello, world!

运行测试



我的测试文件路径是 E:\wamp64\www\my-site\test-php\php\_manual\test\trait\str\_replace.phpt，运行测试命令是



pear run-tests str\_replace.phpt 

运行该目录下的所有测试的命令是



pear run-tests \*.phpt 

测试结果是



E:\wamp64\www\my-site\test-php\php\_manual\test\trait&gt;pear run-tests \*.phpt

Running 2 tests

FAIL sayHello\(\) function

&lt;?php\[2.phpt\]

PASS str\_replace\(\) function\[str\_replace.phpt\]

wrote log to "E:\wamp64\www\my-site\test-php\php\_manual\test\trait\run-tests.log"

TOTAL TIME: 00:00

1 PASSED TESTS

0 SKIPPED TESTS

1 FAILED TESTS:

E:\wamp64\www\my-site\test-php\php\_manual\test\trait\2.phpt

Some tests failed

测试文件格式解析



测试文件标志大小写敏感。要求是大写，如果写成了小写，运行时会报错



Invalid sections formats in test file

–TEST–



--TEST-- 下写明要测试的函数，比如 str\_replace\(\) function，这部分可以随便写，相当于注释。



这部分要写在PHP代码之外，即写在&lt;?php之前。



如何测试类方法，目前不知道。



经测试，测试类的方法与测试非类代码一样。



–FILE–



--FILE-- 下写要测试的代码，它就是正式代码。



注意这部分要加上PHP脚本的结束标志?&gt;。



–EXPECT–



这部分写代码的执行结果。注意，要和执行结果完全一致。



参考资料



.phpt官方文档



php源码测试文件 F:\code\php-7.0.11\tests

