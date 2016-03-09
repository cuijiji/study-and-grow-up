composer 介绍与安装
===
### composer中文网
>http://www.phpcomposer.com/

### Composer 是什么？
>Composer 是一个PHP的依赖管理工具

### composer官网：

>https://getcomposer.org/


## 前提条件

>###php5.3.2+   扩展openssl

## 安装参考网站

>https://getcomposer.org/download/

## 全局安装

	$ curl -sS https://getcomposer.org/installer | php
	$ mv composer.phar /usr/local/bin/composer

### 这里如果你没有把PHP添加到环境变量会报错，你可以用绝对路径或者用下面的命令将PHP添加到环境变量

	$ vi ~/.bash_profile

修改文件的这行代码

	PATH=$PATH:$HOME/bin
后面加上

	:/usr/local/php/bin
