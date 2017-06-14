# 编写php扩展

在ext目录下有ext\_\_skel 和ext\_skel\_\_win32.php

，执行：./ext\_skel --extname=test，会在ext目录下生成test目录，此目录下建立了扩展名称为test的开发框架



3. 进入test目录，编辑config.m4文件



将如下行的注释标签"dnl"去掉，修改后如下所示：



PHP\_ARG\_ENABLE\(myfunctions, whether to enable myfunctions support,



Make sure that the comment is aligned:



\[  --enable-myfunctions           Enable myfunctions support\]\)



4. 使用phpize生成configure文件（phpize路径为：/Applications/XAMPP/xamppfiles/bin/phpize）



5. 执行命令：./configure --with-php-config=/Applications/XAMPP/xamppfiles/bin/php-config



6. 执行命令: make编译扩展



7. 执行命令：sudo make install 安装扩展



8. 修改php.ini文件（路径：/Applications/XAMPP/xamppfiles/etc/php.ini）



9. 重启apache，依次执行一下命令：



sudo /Applications/XAMPP/xamppfiles/apache2/scripts/ctl.sh stop



sudo /Applications/XAMPP/xamppfiles/apache2/scripts/ctl.sh start

