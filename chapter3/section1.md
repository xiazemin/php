# phpt测试

$      vi str\_replace.phpt

$ /Users/didi/pear/pear run-tests str\_replace.phpt

Running 1 tests

FAIL str\_replace\(\) function\[str\_replace.phpt\]

wrote log to "/Users/didi/PHP/run-tests.log"

TOTAL TIME: 00:00

0 PASSED TESTS

0 SKIPPED TESTS

1 FAILED TESTS:

str\_replace.phpt

Some tests failed

phpt文件用于PHP的自动化测试，这是php用自己来测试自己的测试数据用例文件。测试脚本通过执行PHP源码根目录下的run-tests.php，读取phpt文件执行测试。

phpt文件包含 TEST，FILE，EXPECT 等多个段落的文件。在各个段落中，TEST、FILE、EXPECT是基本的段落，每个测试脚本都必须至少包括这三个段落。其中：

TEST段可以用来填写测试用例的名字。

FILE段是一个 PHP 脚本实现的测试用例。

EXPECT段则是测试用例的期待值。

在这三个基本段落之外，还有多个段落，如作为用例输入的GET、POST、COOKIE等，此类字段最终会赋值给$env变量。比如，cookie存放在$env\['HTTP\_COOKIE'\]，$env变量将作为用例中脚本的执行环境。一些主要段落说明如下表所示：

PHP测试脚本中的段落说明

段落名	填充内容	备注

TEST	测试用例名称	必填段落

FILE	测试脚本语句	必填段落。用PHP语言书写的脚本语句。其执行的结果将与 EXPECT\* 段的期待结果做对比。

ARGS	FILE 段的输入参数	选填段落

SKIPIF	跳过这个测试的条件	选填段落

POST	传入测试脚本的 POST 变量	选填段落。如果使用POST段，建议配合使用SKIPIF段

GET	传入测试脚本的 GET 变量	选填段落。如果使用GET段，建议配合使用SKIPIF段。

POST\_RAW	传入测试脚本的POST内容的原生值	选填段落。比如在做文件上传测试时就需要使用此字段来模拟HTTP的POST请求。

COOKIE	传入测试脚本的COOKIE的值	选填段落。最常见的是将PHPSESSID的值传入。

INI	应用于测试脚本的 ini 设置	选填段落。例如 foo=bar 。其值可通过函数 ini\_get\(string name\_entry\) 获得。

ENV	应用于测试脚本的环境设置	选填段落。例如做gzip测试，则需要设置环境HTTP\_ACCEPT\_ENCODING=gzip。

EXPECT	测试脚本的预期结果 相当于测试文件的结果	必填段落

EXPECTF	测试脚本的预期结果	选填段落。可用函数 sscanf\(\) 中的格式表达预期结果 EXPECT 段的变体

EXPECTREGEX	测试脚本的正则预期结果	选填段落。以正则的方式包含多个预期结果，是预期结果EXPECT段的一种变体。

EXPECTHEADERS	测试脚本的预期头部内容	选填段落.测试脚本期待HTTP头部返回，是预期结果EXPECT段的另一种格式。验证过程中会按头部的字段一一比对测试，比如zlib扩展中，如果开启zlib.output\_compression，则在EXPECTHEADERS中包含Content-Encoding: gzip作为预期结果。

phpt文件只是用例文件，它还需要一个控制器来调用这些文件，以实现整个测试过程。PHP的测试控制器文件是源码根目录下的run-tests.php文件。此文件的作用是根据传入的参数，分析用例相关数据，执行测试过程。其大概过程如下：

分析输入的命令行，根据参数配置相关参数，初始化各种信息。

分析用例输入参数，获取需要执行的用例文件列表。PHP支持指定单文件用例执行，支持多文件用例执行，支持\* .phpt多用例执行，支持\* .phpt简化版本多用例执行（相当于.phpt）。

遍历用例文件列表，执行每一个用例。对于每个用例，PHP会具体解析测试脚本中各个段落的含义，清除所有上次测试的记录与设置将准备此次的测试环境，并把各种中间文件和日志文件准备好，然后用环境变量 TEST\_PHP\_EXECUTABLE 指定的 PHP 可执行对象运行实际的测试语句。最后将运行后的结果和测试脚本中的预期结果（EXPECT\*段）进行比较，如果比较结果一致，则测试通过；如果不一致，则测试失败，最后将结果信息一一记录到用户设置的日志文件中。

生成测试结果。

这仅仅是执行的过程，除此之外，还有若干准备和清理工作，如，对上次测试遗留下的环境的清理，本次测试所必须的环境变量的读取与设置，对测试参数的解析，测试脚本名的解析，各种输出文件的准备等等

以测试脚本/tests/basic/001.phpt为例：

\[php\]

--TEST--

Trivial "Hello World" test

--FILE--

&lt;?php echo "Hello World"?&gt;

--EXPECT--

Hello World

这个用例脚本只包含必填的三项。测试控制器会执行--FILE--下面的PHP文件，如果最终的输出是--EXPECT--所期望的结果则表示这个测试通过，如果不一致，则测试不通过，最终这个用例的测试结果会汇总会所有的测试结果集中。



