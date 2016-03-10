title: 性能压力测试工具ab
date: 2016-02-19 16:02:52
tags:
- ab
categories:
- linux
---

### 1、命令原理
ab是apachebench命令的缩写

ab命令会创建很多的并发访问线程，模拟多个访问者同时对某一URL地址进行访问。它的测试目标是基于URL的，因此可以测试各种web服务器如apache、nginx、IIS等的压力。

ab命令对发出负载的计算机要求很低，不会占用很高的CPU和内存，但却可以给目标服务器巨大的负载，原理类似于CC，自己测试时需注意。

### 2、命令的安装
ab的安装非常简单，安装apache之后默认就有，源码安装的apache命令目录为/usr/local/apache2/bin/ab。

> 如果不想安装apache但是又想使用ab命令的话，我们可以直接安装apache的工具包httpd-tools。yum -y install httpd-tools

### 3、ab性能指标
在进行性能测试过程中有几个指标比较重要

* 1、吞吐率（Requests per second）

> 服务器并发处理能力的量化描述，单位是reqs/s，指的是在某个并发用户数下单位时间内处理的请求数。某个并发用户数下单位时间内能处理的最大请求数，称之为最大吞吐率。

> 记住：吞吐率是基于并发用户数的。这句话代表了两个含义：

> a、吞吐率和并发用户数相关

> b、不同的并发用户数下，吞吐率一般是不同的

> 计算公式：总请求数/处理完成这些请求数所花费的时间，即

> Request per second=Complete requests/Time taken for tests

> 必须要说明的是，这个数值表示当前机器的整体性能，值越大越好。

* 2、并发连接数（The number of concurrent connections）

> 并发连接数指的是某个时刻服务器所接受的请求数目，简单的讲，就是一个会话。

* 3、并发用户数（Concurrency Level）

> 要注意区分这个概念和并发连接数之间的区别，一个用户可能同时会产生多个会话，也即连接数。在HTTP/1.1下，IE7支持两个并发连接，IE8支持6个并发连接，FireFox3支持4个并发连接，所以相应的，我们的并发用户数就得除以这个基数。

* 4、用户平均请求等待时间（Time per request）

> 计算公式：处理完成所有请求数所花费的时间/（总请求数/并发用户数），即：

> Time per request=Time taken for tests/（Complete requests/Concurrency Level）

* 5、服务器平均请求等待时间（Time per request:across all concurrent requests）

> 计算公式：处理完成所有请求数所花费的时间/总请求数，即：

> Time taken for/testsComplete requests

> 可以看到，它是吞吐率的倒数。

> 同时，它也等于用户平均请求等待时间/并发用户数，即

> Time per request/Concurrency Level


### 4、参数说明
格式： ab options http:// hostname :port /path

参数详解

| 参数 | 值 | 解释| 中文解释|
| -----|----|-----|-----|
| -n | requests  |   Number of requests to perform | 在测试会话中所执行的请求个数（本次测试总共要访问页面的次数）。默认时，仅执行一个请求。|
|-c  | concurrency   |   Number of multiple requests to make | 一次产生的请求个数（并发数）。默认是一次一个 |
|-t  | timelimit    |    Seconds to max. wait for responses | 测试所进行的最大秒数。其内部隐含值是-n 50000。它可以使对服务器的测试限制在一个固定的总时间以内。默认时，没有时间限制。 |
|-b  | windowsize   |    Size of TCP send/receive buffer, in bytes | |
|-p  | postfile     |    File containing data to POST. Remember also to set -T |包含了需要POST的数据的文件，文件格式如“p1=1&p2=2”.使用方法是 -p 111.txt 。 （配合-T）|
|-u  | putfile      |    File containing data to PUT. Remember also to set -T ||
|-T  | content-type |  Content-type header for POSTing, eg.'application/x-www-form-urlencoded'Default is 'text/plain' | POST数据所使用的Content-type头信息，如 -T “application/x-www-form-urlencoded” 。 （配合-p）|
|-v  | verbosity    |    How much troubleshooting info to print |设置显示信息的详细程度 – 4或更大值会显示头信息， 3或更大值可以显示响应代码(404, 200等), 2或更大值可以显示警告和其他信息。 -V 显示版本号并退出。|
|-w   |             |    Print out results in HTML tables | 以HTML表的格式输出结果。默认时，它是白色背景的两列宽度的一张表。 |
|-i   |             |    Use HEAD instead of GET |  执行HEAD请求，而不是GET。|
|-x  | attributes   |    String to insert as table attributes | |
|-y  | attributes   |    String to insert as tr attributes | |
|-z  | attributes   |    String to insert as td or th attributes | |
|-C  | attribute    |    Add cookie, eg. 'Apache=1234. (repeatable) | -C cookie-name=value 对请求附加一个Cookie:行。其典型形式是name=value的一个参数对。此参数可以重复，用逗号分割。提示：可以借助session实现原理传递 JSESSIONID参数， 实现保持会话的功能，如-C ” c1=1234,c2=2,c3=3, JSESSIONID=1111 。 |
|-H  | attribute    |    Add Arbitrary header line, eg. 'Accept-Encoding: gzip'Inserted after all normal header lines. (repeatable) | |
|-A  | attribute    |    Add Basic WWW Authentication, the attributes are a colon separated username and password. | |
|-P  | attribute   |     Add Basic Proxy Authentication, the attributes are a colon separated username and password. | proxy-auth-username:password 对一个中转代理提供BASIC认证信任。用户名和密码由一个:隔开，并以base64编码形式发送。无论服务器是否需要(即, 是否发送了401认证需求代码)，此字符串都会被发送。 |
|-X  | proxy:port |   Proxyserver and port number to use | |
|-V   |            |  Print version number and exit | |
|-k   |           |   Use HTTP KeepAlive feature | |
|-d   |           |   Do not show percentiles served table. | |
|-S   |           |   Do not show confidence estimators and warnings. | |
|-g  | filename   |   Output collected data to gnuplot format file. | |
|-e  | filename   |   Output CSV file with percentages served | |
|-r    |          |   Don't exit on socket receive errors. | |
|-h  |           |    Display usage information (this message) | 设置属性的字符串. 缺陷程序中有各种静态声明的固定长度的缓冲区。另外，对命令行参数、服务器的响应头和其他外部输入的解析也很简单，这可能会有不良后果。它没有完整地实现 HTTP/1.x; 仅接受某些’预想’的响应格式。 strstr(3)的频繁使用可能会带来性能问题，即你可能是在测试ab而不是服务器的性能。|
|-Z  | ciphersuite|   Specify SSL/TLS cipher suite (See openssl ciphers) | |
|-f  | protocol   |   Specify SSL/TLS protocol (SSL2, SSL3, TLS1, or ALL) | |


### 5、用法详解
ab的参数很多，一般我们用-c 和-n参数就可以了，例如：
> -n后面的4000代表总共发出4000个请求，-c后面的1000表示采用1000个并发（模拟1000个人同时访问），后面的地址表示测试的目标URL。

```
ab -n 4000 -c 1000 http://192.168.35.2/
```

稍等一会得到类似如下的显示结果：
```
[root@vagrant-centos65 ~]# ab -n 4000 -c 1000 http://192.168.35.2/
This is ApacheBench, Version 2.3 <$Revision: 655654 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 192.168.35.2 (be patient)
Completed 400 requests
Completed 800 requests
Completed 1200 requests
Completed 1600 requests
Completed 2000 requests
Completed 2400 requests
Completed 2800 requests
Completed 3200 requests
Completed 3600 requests
Completed 4000 requests
Finished 4000 requests

Server Software:        nginx/1.4.2
Server Hostname:        192.168.35.2
Server Port:            80

Document Path:          /
Document Length:        612 bytes

Concurrency Level:      1000
Time taken for tests:   3.279 seconds
Complete requests:      4000
Failed requests:        1343
   (Connect: 0, Receive: 0, Length: 1343, Exceptions: 0)
Write errors:           0
Non-2xx responses:      1343
Total transferred:      3131835 bytes
HTML transferred:       2185044 bytes
Requests per second:    1219.91 [#/sec] (mean)
Time per request:       819.731 [ms] (mean)
Time per request:       0.820 [ms] (mean, across all concurrent requests)
Transfer rate:          932.76 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        1  329 463.5     72    1314
Processing:    18  185 156.3    163    1433
Waiting:        1  181 157.0    157    1433
Total:         19  515 576.9    271    2594

Percentage of the requests served within a certain time (ms)
  50%    271
  66%    373
  75%   1026
  80%   1085
  90%   1679
  95%   1767
  98%   1848
  99%   1855
 100%   2594 (longest request)
```


结果分析
```
[root@vagrant-centos65 ~]# ab -n 4000 -c 1000 http://192.168.35.2/
This is ApacheBench, Version 2.3 <$Revision: 655654 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 192.168.35.2 (be patient)
Completed 400 requests
Completed 800 requests
Completed 1200 requests
Completed 1600 requests
Completed 2000 requests
Completed 2400 requests
Completed 2800 requests
Completed 3200 requests
Completed 3600 requests
Completed 4000 requests
Finished 4000 requests

# 服务器软件版本
Server Software:        nginx/1.4.2
# hostname
Server Hostname:        192.168.35.2
# 端口
Server Port:            80

# 测试的页面
Document Path:          /
# 页面大小
Document Length:        612 bytes

# 测试的并发数
Concurrency Level:      1000
# 整个测试持续的时间
Time taken for tests:   3.279 seconds
# 完成的请求数量
Complete requests:      4000
# 失败的请求数量
Failed requests:        1343
   (Connect: 0, Receive: 0, Length: 1343, Exceptions: 0)
Write errors:           0
Non-2xx responses:      1343
# 整个过程中的网络传输量
Total transferred:      3131835 bytes
# 整个过程中的HTML内容传输量
HTML transferred:       2185044 bytes
# 最重要的指标之一，每秒请求数，后面括号中的mean表示这是一个平均值
Requests per second:    1219.91 [#/sec] (mean)
# 最重要的指标之二，平均请求响应时间
Time per request:       819.731 [ms] (mean)
# 每个连接请求实际运行时间的平均值
Time per request:       0.820 [ms] (mean, across all concurrent requests)
# 平均每秒网络上的流量，可以帮助排除是否存在网络流量过大导致响应时间延长的问题
Transfer rate:          932.76 [Kbytes/sec] received

# 网络上消耗时间的分解
Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        1  329 463.5     72    1314
Processing:    18  185 156.3    163    1433
Waiting:        1  181 157.0    157    1433
Total:         19  515 576.9    271    2594

# 整个场景中所有请求的响应时间占比。
Percentage of the requests served within a certain time (ms)
  50%    271
  66%    373
  75%   1026
  80%   1085
  90%   1679
  95%   1767
  98%   1848
  99%   1855
 100%   2594 (longest request)

 ```
### 参考：
* [Web性能压力测试工具之ApacheBench（ab）详解](http://www.ha97.com/4617.html)
* [apache性能测试工具ab的应用](http://www.ilanni.com/?p=8348)
