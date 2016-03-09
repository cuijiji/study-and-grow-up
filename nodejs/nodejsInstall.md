LINUX下安装nodejs
--
###下载（你也可以去官网自行下载）

	$ wget https://nodejs.org/dist/v4.3.1/node-v4.3.1-linux-x64.tar.xz

###然后解压

	$ xz -d xxx.tar.xz 
	$ tar xvf xxx.tar
###最后自行软连到bin下面

	$ ln -s /root/src/node-v4.3.1-linux-x64/bin/node /usr/local/bin/node
	$ ln -s /root/src/node-v4.3.1-linux-x64/bin/npm /usr/local/bin/npm
###测试

	$ node -v

###如果出现版本号你成功了（如果不成功看看路径改成你自己的）