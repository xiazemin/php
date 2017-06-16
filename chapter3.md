# 安装PEAR

教程：[https://jason.pureconcepts.net/2012/10/install-pear-pecl-mac-os-x/](https://jason.pureconcepts.net/2012/10/install-pear-pecl-mac-os-x/)

$  curl -O [http://pear.php.net/go-pear.phar](http://pear.php.net/go-pear.phar)

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current

```
                             Dload  Upload   Total   Spent    Left  Speed
```

100 3510k  100 3510k    0     0   331k      0  0:00:10  0:00:10 --:--:--  730k

$ php -d detect\_unicode=0 go-pear.phar

Below is a suggested file layout for your new PEAR installation.  To

change individual locations, type the number in front of the

directory.  Type 'all' to change all of them or simply press Enter to

accept these locations.

1. Installation base \($prefix\)                   : /Users/didi/pear

2. Temporary directory for processing            : /tmp/pear/install

3. Temporary directory for downloads             : /tmp/pear/install

4. Binaries directory                            : /Users/didi/pear/bin

5. PHP code directory \($php\_dir\)                 : /Users/didi/pear/share/pear

6. Documentation directory                       : /Users/didi/pear/docs

7. Data directory                                : /Users/didi/pear/data

8. User-modifiable configuration files directory : /Users/didi/pear/cfg

9. Public Web Files directory                    : /Users/didi/pear/www

1. System manual pages directory                 : /Users/didi/pear/man

2. Tests directory                               : /Users/didi/pear/tests

3. Name of configuration file                    : /Users/didi/.pearrc

1-12, 'all' or Enter to continue: 1

\(Use $prefix as a shortcut for '/Users/didi/pear', etc.\)

Installation base \($prefix\) \[/Users/didi/pear\] : /Users/didi/pear

Below is a suggested file layout for your new PEAR installation.  To

change individual locations, type the number in front of the

directory.  Type 'all' to change all of them or simply press Enter to

accept these locations.

1. Installation base \($prefix\)                   : /Users/didi/pear

2. Temporary directory for processing            : /tmp/pear/install

3. Temporary directory for downloads             : /tmp/pear/install

4. Binaries directory                            : /Users/didi/pear/bin

5. PHP code directory \($php\_dir\)                 : /Users/didi/pear/share/pear

6. Documentation directory                       : /Users/didi/pear/docs

7. Data directory                                : /Users/didi/pear/data

8. User-modifiable configuration files directory : /Users/didi/pear/cfg

9. Public Web Files directory                    : /Users/didi/pear/www

1. System manual pages directory                 : /Users/didi/pear/man

2. Tests directory                               : /Users/didi/pear/tests

3. Name of configuration file                    : /Users/didi/.pearrc

1-12, 'all' or Enter to continue: 4

\(Use $prefix as a shortcut for '/Users/didi/pear', etc.\)

Binaries directory \[$prefix/bin\] : /Users/didi/pear

Below is a suggested file layout for your new PEAR installation.  To

change individual locations, type the number in front of the

directory.  Type 'all' to change all of them or simply press Enter to

accept these locations.

1. Installation base \($prefix\)                   : /Users/didi/pear

2. Temporary directory for processing            : /tmp/pear/install

3. Temporary directory for downloads             : /tmp/pear/install

4. Binaries directory                            : /Users/didi/pear

5. PHP code directory \($php\_dir\)                 : /Users/didi/pear/share/pear

6. Documentation directory                       : /Users/didi/pear/docs

7. Data directory                                : /Users/didi/pear/data

8. User-modifiable configuration files directory : /Users/didi/pear/cfg

9. Public Web Files directory                    : /Users/didi/pear/www

1. System manual pages directory                 : /Users/didi/pear/man

2. Tests directory                               : /Users/didi/pear/tests

3. Name of configuration file                    : /Users/didi/.pearrc

1-12, 'all' or Enter to continue:

Beginning install...

Configuration written to /Users/didi/.pearrc...

Initialized registry...

Preparing to install...

installing phar:///Users/didi/PHP/go-pear.phar/PEAR/go-pear-tarballs/Archive\_Tar-1.4.2.tar...

installing phar:///Users/didi/PHP/go-pear.phar/PEAR/go-pear-tarballs/Console\_Getopt-1.4.1.tar...

installing phar:///Users/didi/PHP/go-pear.phar/PEAR/go-pear-tarballs/PEAR-1.10.4.tar...

installing phar:///Users/didi/PHP/go-pear.phar/PEAR/go-pear-tarballs/Structures\_Graph-1.1.1.tar...

installing phar:///Users/didi/PHP/go-pear.phar/PEAR/go-pear-tarballs/XML\_Util-1.4.2.tar...

install ok: channel://pear.php.net/Archive\_Tar-1.4.2

install ok: channel://pear.php.net/Console\_Getopt-1.4.1

install ok: channel://pear.php.net/Structures\_Graph-1.1.1

install ok: channel://pear.php.net/XML\_Util-1.4.2

install ok: channel://pear.php.net/PEAR-1.10.4

PEAR: Optional feature webinstaller available \(PEAR's web-based installer\)

PEAR: Optional feature gtkinstaller available \(PEAR's PHP-GTK-based installer\)

PEAR: Optional feature gtk2installer available \(PEAR's PHP-GTK2-based installer\)

PEAR: To install optional features use "pear install pear/PEAR\#featurename"

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

WARNING!  The include\_path defined in the currently used php.ini does not

contain the PEAR PHP directory you just specified:

&lt;/Users/didi/pear/share/pear&gt;

If the specified directory is also not in the include\_path used by

your scripts, you will have problems getting any PEAR packages working.

Would you like to alter php.ini &lt;/usr/local/etc/php/7.0/php.ini&gt;? \[Y/n\] : Y

php.ini &lt;/usr/local/etc/php/7.0/php.ini&gt; include\_path updated.

Current include path           : .:

Configured directory           : /Users/didi/pear/share/pear

Currently used php.ini \(guess\) : /usr/local/etc/php/7.0/php.ini

Press Enter to continue:

\*\* WARNING! Old version found at /Users/didi/pear, please remove it or be sure to use the new /Users/didi/pear/pear command

The 'pear' command is now at your service at /Users/didi/pear/pear

\*\* The 'pear' command is not currently in your PATH, so you need to

\*\* use '/Users/didi/pear/pear' until you have added

\*\* '/Users/didi/pear' to your PATH environment variable.

Run it without parameters to see the available actions, try 'pear list'

to see what packages are installed, or 'pear help' for help.

For more information about PEAR, see:

[http://pear.php.net/faq.php](http://pear.php.net/faq.php)

[http://pear.php.net/manual/](http://pear.php.net/manual/)

Thanks for using go-pear!

$ /Users/didi/pear/pear version

PEAR Version: 1.10.4

PHP Version: 7.0.15

Zend Engine Version: 3.0.0

Running on: Darwin localhost 15.0.0 Darwin Kernel Version 15.0.0: Sat Sep 19 15:53:46 PDT 2015; root:xnu-3247.10.11~1/RELEASE\_X86\_64 x86\_64

