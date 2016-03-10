title: linux下svn服务器搭建
date: 2016-03-02 16:57:47
tags:
- svn
categories:
- linux
---

## 1、准备工作

```
wget http://mirrors.cnnic.cn/apache/subversion/subversion-1.8.13.tar.gz
wget http://apache.fayea.com//httpd/httpd-2.4.16.tar.gz
wget http://apache.fayea.com//apr/apr-1.5.2.tar.gz
wget http://apache.fayea.com//apr/apr-util-1.5.4.tar.gz
wget http://sqlite.org/2015/sqlite-autoconf-3081002.tar.gz
wget http://exim.mirror.fr/pcre/pcre-8.34.tar.gz
```

## 2、安装Apr和Apr-util
```
tar  zxf apr-1.5.0.tar.gz
./configure --prefix=/usr/local/apr
make && make install

tar zxf apr-util-1.5.3.tar.bz2
  ./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr/bin/apr-1-config
make && make  install
```

## 3、安装Pcre 和Apache

```
tar zxf pcre-8.34.zip
./configure --prefix=/usr/local/pcre
tar  zxf httpd-2.4.7.tar.gz
make&&make install

tar zxvf http httpd-2.4.7.tar.gz
./configure --prefix=/usr/local/apache --enable-so --enable-dav --enable-dav-fs --enable-ssl --enable-maintainer-mode --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util --with-pcre=/usr/local/pcre
make && make install
# 启动apache
/usr/local/apache/bin/apachectl start
```


提示：如果出现
checking whether to enable mod_ssl... configure: error: mod_ssl has been requested but can not be built due to prerequisite failures
yum install openssl-devel


提示：如果出现
configure: error: We require OpenSSL; try --with-openssl
解决方法：

```
wget http://www.openssl.org/source/openssl-1.0.0a.tar.gz
tar -zxvf openssl- openssl-1.0.1f.tar.gz
cd openssl- openssl-1.0.1f
./config -fPIC --prefix=/usr/local/openssl/ enable-shared
make && make install
```


注：如果缺失--enable-dav，启动apache时会报“httpd: Syntax error on line xxx of /usr/local/apache/conf/httpd.conf: Cannot load /usr/local/apache2/modules/mod_dav_svn.so into server: /usr/local/apache2/modules/mod_dav_svn.so: undefined symbol: dav_register_provider”需要重新编译安装才能解决


## 4、安装Sqlite

```
tar zxf sqlite-autoconf-3080301.tar.gz
./configure
make && make install
```

提示：configure配置SVN时可能提示如下错误信息：
configure: error: subversion requires zlib
错误提示需要安装zlib
下载zlb：http://zlib.net/

```
tar -xvzf zlib-1.2.5.tar.gz
cd zlib-1.2.5
./configure
make && make install
```
## 5、安装Subversion

```
tar zxf subversion-1.8.8.tar.gz
./configure --prefix=/usr/local/subversion --with-apxs=/usr/local/apache/bin/apxs --with-apr=/usr/local/apr/  --with-apr-util=/usr/local/apr-util/ --with-openssl --with-zlib --enable-maintainer-mode
make && make install
# 查看是否安装成功
/usr/local/subversion/bin/svnserve —version
```

提示：在以上编译过程中可能会出现configure: error: Subversion requires SQLite
可以wget http://www.sqlite.org/sqlite-amalgamation-3071501.zip
安装Sqlite
unzip sqlite-amalgamation-3071501.zip
进入文件夹sqlite-amalgamation找到sqlite3.c
将其复制到sqlite-autoconf-3080301 /sqlite3.c
    重新执行上面的./configure 及make、make install

## 6、相关配置

### 6.1、配置subversion

拷贝相关库

```
cp /usr/local/subversion/libexec/mod_authz_svn.so  /usr/local/apache/modules/
cp /usr/local/subversion/libexec/mod_dav_svn.so  /usr/local/apache/modules/
```


### 6.2添加svn的验证文件svn-auth.conf

```
cd /usr/local/subversion
mkdir conf

#/usr/local/apache/bin/htpasswd -cm  /usr/local/subversion/conf/auth.conf admin
#New password:
#Re-type new password:
#Adding password for user getprod
注：只有第一次需要加-c参数，后期再添加用戶千万不能加-c，否则会将原文件覆盖，如下：
#/usr/local/apache/bin/htpasswd -m ~/subversion/svn/auth.conf getprodone New password:
#Re-type new password:
#Adding password for user getprodone
```

### 6.3  配置svn权限控制文件
```
vim /usr/local/subversion/svn/svn-access.conf
[groups]
rd = getprod,getprodone    #passwd文件中存在的用户，rd代表用户组，在组上赋权限  #qa =   #同上
[/]    #对svn库repo的根目录赋权，但随svn启动命令不同，写的不同
@rd = rw    #对rd组赋读写权限
#@qa = r
```

注：[groups]如果少了字符如[group]，则用浏览器登录时，只能看到“Collection of Repositories”

### 6.4、配置apache

添加apache专属用户www：

useradd  www

添加文夹 ：

```
mkdir  /www
# vim /usr/local/apache/conf/httpd.conf     查看apache httpd.conf文件，添加  LoadModule dav_svn_module modules/mod_dav_svn.so
LoadModule authz_svn_module modules/mod_authz_svn.so

修改：
User www
Group www

修改：
   DocumentRoot "/www"
   <Directory "/www">

    修改：
<VirtualHost *:80>
   # ServerAdmin webmaster@dummy-host.example.com
    DocumentRoot /www
    ServerName test208.com
    DirectoryIndex index.jsp index.html index.htm
  #  ServerAlias www.dummy-host.example.com
  #  ErrorLog "logs/dummy-host.example.com-error_log"
  #  CustomLog "logs/dummy-host.example.com-access_log" common
     ErrorDocument 404 /404.php
     ErrorDocument 403 /403.php
</VirtualHost>

    添加：
<Location /svn>
DAV svn
SVNParentPath /data/svn
SVNListParentPath on
AuthType Basic
AuthName "Subversion repository"
AuthUserFile "/usr/local/subversion/svn/auth.conf"
Require valid-user
AuthzSVNAccessFile "/usr/local/subversion/svn/svn-acess.conf"
</Location>
```

### 6.5 新建第一个库
添加文件加：
```
mkdir  -p  /data/svn/repo
chown  -R  www:www /data/svn/repo
svnadmin create /data/svn/repo
```


启动svnserve：
```
svnserve -d -r /data/svn
```

### 6.6启动Apache：
```
/usr/local/apache/bin/apachectl start

提示： 如果报“httpd: Syntax error on line xxx of /usr/local/apache/conf/httpd.conf: Cannot load /usr/local/apache2/modules/mod_dav_svn.so into server: /usr/local/apache2/modules/mod_dav_svn.so: undefined symbol: dav_register_provider”
可打开httpd.conf
vi /usr/local/apache/conf/httpd.conf
将#LoadModule dav_module modules/mod_dav.so前面的#去掉即可

设置svn 和apache 服务开机自动启用
#vi /etc/rc.local
/usr/local/apache/bin/apachectl start
svnserve –d –r /data/svn
```

在浏览器地址栏中输入http：//svnserverIP/svn

还参考了
http://www.cnblogs.com/fyfblog/archive/2011/06/12/2097138.html
