# 编写php扩展

在ext目录下有ext\_\_skel 和ext\_skel\_\_win32.php,用于生成开发扩展的工程

执行：./ext\_skel --extname=test，会在ext目录下生成test目录，此目录下建立了扩展名称为test的开发框架

$     ./ext\_skel --extname=test

Creating directory test

Creating basic files: config.m4 config.w32 .gitignore test.c php\_test.h CREDITS EXPERIMENTAL tests/001.phpt test.php \[done\].

To use your new extension, you will have to execute the following steps:

1. $ cd ..

2. $ vi ext/test/config.m4

3. $ ./buildconf

4. $ ./configure --\[with\|enable\]-test

5. $ make

6. $ ./sapi/cli/php -f ext/test/test.php

7. $ vi ext/test/test.c

8. $ make

Repeat steps 3-6 until you are satisfied with ext/test/config.m4 and

step 6 confirms that your module is compiled into PHP. Then, start writing

code and repeat the last two steps as often as necessary.

1. 进入test目录，编辑config.m4文件

$ ls

CREDITS        config.m4    php\_test.h    test.php

EXPERIMENTAL    config.w32    test.c        tests

$ vi config.m4将如下行的注释标签"dnl"去掉，

~~dnl~~ PHP\_ARG\_ENABLE\(test, whether to enable test support,

~~dnl~~ Make sure that the comment is aligned:

~~dnl~~ \[ --enable-test Enable test support\]\)

修改后如下所示：

PHP\_ARG\_ENABLE\(myfunctions, whether to enable myfunctions support,

Make sure that the comment is aligned:

\[  --enable-myfunctions           Enable myfunctions support\]\)

1. 使用phpize生成configure文件（phpize路径为：/Applications/XAMPP/xamppfiles/bin/phpize）

$ phpize

Configuring for:

PHP Api Version:         20151012

Zend Module Api No:      20151012

Zend Extension Api No:   320151012

$ ls

CREDITS        aclocal.m4    config.h.in    configure    missing        test.c

EXPERIMENTAL    autom4te.cache    config.m4    configure.in    mkinstalldirs    test.php

Makefile.global    build        config.sub    install-sh    php\_test.h    tests

acinclude.m4    config.guess    config.w32    ltmain.sh    run-tests.php

1. 执行命令：./configure --with-php-config=/Applications/XAMPP/xamppfiles/bin/php-config

$ ./configure

checking for grep that handles long lines and -e... /usr/bin/grep

checking for egrep... /usr/bin/grep -E

checking for a sed that does not truncate output... /usr/bin/sed

checking for cc... cc

checking whether the C compiler works... yes

1. 执行命令: make编译扩展

2. make test

3. 执行命令：sudo make install 安装扩展

$ make install

Installing shared extensions:     /usr/local/Cellar/php70/7.0.15\_8/lib/php/extensions/no-debug-non-zts-20151012/

1. 修改php.ini文件（路径：/Applications/XAMPP/xamppfiles/etc/php.ini）

   vi /usr/local/etc/php/7.0/php.ini

extension\_dir = "/usr/local/Cellar/php70/7.0.15\_8/lib/php/extensions/no-debug-non-zts-20151012/"

extension=test.so

1. 重启apache，$ sudo nginx  -s  reopen -c /usr/local/etc/nginx/nginx.conf

依次执行一下命令：

sudo /Applications/XAMPP/xamppfiles/apache2/scripts/ctl.sh stop

sudo /Applications/XAMPP/xamppfiles/apache2/scripts/ctl.sh start

$ /usr/local/sbin/php70-fpm  start

![](/assets/importext.png)$   PHP -m \|grep test

