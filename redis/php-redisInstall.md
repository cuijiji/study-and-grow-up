PHP编译Redis扩展
===
> ###在php下使用redis扩展，可用直接编译redis扩展，也可以通过predis包直接操作，这个看情况~


###下载安装包并解压

	$ wget http://pecl.php.net/get/redis-2.2.5.tgz
	$ tar zxvf redis-2.2.5.tgz
###执行下面命令并安装（注意路径改成你自己的）

	/php/path/bin/phpize
	configure --with-php-config=/usr/local/php/bin/php-config
	make && make install
###这时候你会出现下面这样的一行，这就是你redis.so的路径
	/usr/local/php/lib/php/extensions/no-debug-non-zts-20131226/
###找到你的php.ini 然后编辑它 加入这样一行代码(注意，路径变成你自己的)

	$ extension=/usr/local/php/lib/php/extensions/no-debug-non-zts-20131226/redis.so
###重启php-fpm，查看你的phpinfo,如果出现的redis，恭喜你成功了！



