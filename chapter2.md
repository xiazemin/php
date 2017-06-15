# PHP7.1下 vld扩展的安装使用

1）Git clone [https://github.com/derickr/vld.git](https://github.com/derickr/vld.git)

2）cd vld

3）phpize

$ phpize

Configuring for:

PHP Api Version:         20151012

Zend Module Api No:      20151012

Zend Extension Api No:   320151012

4）./configure

$ ./configure

checking for grep that handles long lines and -e... /usr/bin/grep

checking for egrep... /usr/bin/grep -E

5）make && make install

$ make install

Installing shared extensions:     /usr/local/Cellar/php70/7.0.15\_8/lib/php/extensions/no-debug-non-zts-20151012/

6）添加ext-vld.ini配置文件

7）重启fpm

8）PHP -m \| grep vld 查看扩展

9）php -dvld.active test.php 测试vld扩展

关于VLD扩展显示信息的一点点解释

其中：

branch analysis from position 在分析数组时使用

return found是否返还

filename 分析的文件名

function name函数名

number of ops生成的操作数

compiled vars编译期间的变量，PHP5后添加，是一个缓存优化，在PHP源码中以IS\_CV标记

op list生成的中间代码的变量列表

-dvld.active输出的是VLD的默认设置，使用-dvld.verbosity可以查看更加详细的内容

包含各个中间代码的操作数等

若只想看到输出的中间代码，并不想实际执行这段代码，可以使用-dvld.execute = 0来禁用代码的执行

php -dvld.active=1 -dvld.execute=0 test.php

它还可以支持输出.dot文件

php -dvld.active=1 -dvld.save\_dir='D:\tmp' -dvld.save\_paths=1 -dvld.dump\_paths=1 t.php 会将生成的中间代码的信息输出再D:/tmp/path.dot中

-dvld.format是否以自定义的格式输出，默认为否，是指以-dvld.col\_sep指定的参数间隔

-dvld.col\_sep在-dvld.format参数启用时才会有效，默认为 \t

-dvld.verbosity是否显示更加详细的信息，默认为1，其值可以是0，1，2，3 或者小于0只是比1小的效果会喝0一样，负数的效果和3的效果一样

-dvld.save\_dir指定文件的输出路径，默认/tmp

-dvld.save\_path指定文件输出的路径，默认0表示不输出文件

-dvld.dump\_paths控制输出的内容，0或1 默认1，即输出内容

