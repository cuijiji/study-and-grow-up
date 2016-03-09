Redis 详解与安装
======
---------


###_redis是什么:_
>Redis is an open source, BSD licensed, advanced key-value store. It is often referred to as a data structure server since keys can contain strings, hashes, lists, sets and sorted sets.
redis是开源,BSD许可,高级的key-value存储系统.
可以用来存储字符串,哈希结构,链表,集合,因此,常用来提供数据结构服务.

###_redis和memcached相比,的独特之处:_
>1: redis可以用来做存储(storge), 而memccached是用来做缓存(cache)
  这个特点主要因为其有”持久化”的功能.
2: 存储的数据有”结构”,对于memcached来说,存储的数据,只有1种类型--”字符串”,
  而redis则可以存储字符串,链表,哈希结构,集合,有序集合.

##Redis下载安装
###去官方网站http://redis.io/下载即可
	$ wget http://download.redis.io/releases/redis-3.0.7.tar.gz
###然后进入目录解压

	$ cd redis-3.0.7
	$ tar zxvf redis-3.0.7.tar.gz
###启动并运行redis：

	$ ./src/redis-server
###打开另外一个终端运行，redis客户端

	$ ./src/redis-cli

###试着运行下面的代码

	127.0.0.1:6379> set name cuijiji

	OK

	127.0.0.1:6379> get name

	"cuijiji"

	127.0.0.1:6379>
###恭喜你成功了！！！
