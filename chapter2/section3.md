# phpize

phpize的作用可以这样理解：侦测环境\(phpize工具是在php安装目录下,基于这点phpize对应了当时的php环境，所以是要根据该php的配置情况生成对应的configure文件\)，建立一个configure文件。必须在一个目录下去运行phpize。那么phpize就知道你的的环境是哪个目录，并且configure文件建立在该目录下。



步骤总结：



一、cd /usr/src/php源码包目录/ext/扩展目录/



二、/usr/local/php5314/bin/phpize





三、./configure --with-php-config=/usr/local/php5314/bin/php-config



四、make && make install



 



ps:make install会自动将生成的.so扩展复制到php的扩展目录下去，比如会提示已经安装到 /usr/local/php/php-5.5.18/lib/php/extensions/no-debug-non-zts-20121212/目录下去

五、剩下是配置php.ini



 



 





假如你的服务器上安装了多个版本php，那么需要告诉phpize要建立基于哪个版本的扩展。通过使用--with-php-config=指定你使用哪个php版本。



比如：--with-php-config=/usr/local/php524/bin/php-config



关于php-config文件：是在php编译生成后\(安装好\)，放在安装目录下的一个文件。打开phpize文件内容会发现，里面定义好了php的安装目录等变量



prefix='/usr/local/php'



phpize是编译安装时候生成好的，记录了当时安装的一些信息。并不能从其他地方拿个phpize来使用。



phpize是在php安装目录下的一个文件。比如我安装了两个php5.2 和php5.3那么使用phpize也要使用对应版本的phpize才行。此时使用--with-php-config有什么作用？



 



phpize工具一般在哪里？



当php编译完成后，php安装目录下的bin目录下会有phpize这个脚本文件。所以是去安装好的php安装目录去找。

