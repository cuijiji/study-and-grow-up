#Git 介绍与使用
------------------------
##Git简介
###Git是什么？

>**最流行的分布式版本控制系统。**
###Git有什么特点？
>**免费而超级好用。**
##Git安装
###在Linux上安装Git
首先你可以试着输入__git__，看看系统有没有安装Git！

	$ git
	usage: git [--version] [--exec-path[=GIT_EXEC_PATH]] [--html-path]
           [-p|--paginate|--no-pager] [--no-replace-objects]
           [--bare] [--git-dir=GIT_DIR] [--work-tree=GIT_WORK_TREE]
           [--help] COMMAND [ARGS]

	The most commonly used git commands are:
	   add        Add file contents to the index
  	   bisect     Find by binary search the change that 	introduced a bug
  	   branch     List, create, or delete branches
  	   checkout   Checkout a branch or paths to the working 	tree
	   clone      Clone a repository into a new directory
	   commit     Record changes to the repository
	   diff       Show changes between commits, commit and working tree, etc
	   fetch      Download objects and refs from another repository
	   grep       Print lines matching a pattern
	   init       Create an empty git repository or reinitialize an existing one
	   log        Show commit logs
	   merge      Join two or more development histories together
	   mv         Move or rename a file, a directory, or a symlink
	   pull       Fetch from and merge with another repository or a local branch
	   push       Update remote refs along with associated objects
	   rebase     Forward-port local commits to the updated upstream head
	   reset      Reset current HEAD to the specified state
	   rm         Remove files from the working tree and from the index
	   show       Show various types of objects
	   status     Show the working tree status
	   tag        Create, list, delete or verify a tag object signed with GPG
	
	See 'git help COMMAND' for more information on a specific command.
像上面的命令,如果是这样的证明你的系统已然存在git 不需要去安装了
	
	$ git
	The program 'git' is currently not installed. You can install it by typing:
	sudo apt-get install git
如果碰到这种情况，有很多Linux会友好地告诉你Git没有安装，还会告诉你如何安装Git。别慌，现在我来带你安装git!


如果你碰巧用Debian或Ubuntu Linux，运行下面这条命令就可以直接完成Git的安装，非常简单。

	$ sudo apt-get install git

如果你的Debian或Ubuntu Linux太老了，那试试下面这条命令！

	$ sudo apt-get install git-core
如果是其他Linux版本，可以直接通过源码安装。先从Git官网下载源码，然后解压，依次输入：**./config，make，sudo make install**这几个命令安装就好了。

###在Mac OS X上安装Git
如果你正在使用Mac做开发，你可能很有钱请联系我<**QQ624337224**>！
下面我提供两种安装Git的方法,这里我们推荐使用第二种！

+	安装homebrew，然后通过homebrew安装Git，具体方法请参考homebrew的文档：<http://brew.sh/>。
+	从AppStore安装Xcode，Xcode集成了Git，不过默认没有安装，你需要运行Xcode，选择菜单“Xcode”->“Preferences”，在弹出窗口中找到“Downloads”，选择“Command Line Tools”，点“Install”就可以完成安装了。

**Xcode是Apple官方IDE，功能非常强大，是开发Mac和iOS App的必选装备，而且是免费的！**

###在Windows上安装Git
说实话**windows**是非常垃圾的开发平台，个人不推荐。但是好多人都用这款操作系统，我们还是说一下吧。

**msysgit是Windows版的Git，点击下面这条链接下载到本地！**

><http://dlsw.baidu.com/sw-search-sp/soft/4e/30195/Git_V2.5.1_64_bit_setup.1441791170.exe>

我们使用的是Git Bash。去你的应用里面打开这个，会出现一个类似cmd的黑色窗口，恭喜你，成功安装**windows**版本**git**！

以上安装完成后，还需要最后一步设置，在命令行输入：

	$ git config --global user.name "Your Name"
	$ git config --global user.email "email@example.com"
>注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址

>例如如果你公司的项目是放在自建的gitlab上面, 如果你不进行配置用户名和邮箱的话, 则会使用全局的, 这个时候是错误的, 正确的做法是针对公司的项目, 在项目根目录下进行单独配置
	
	$ git config user.name "Your Name"
	$ git config user.email "email@example.com"
那我们应该怎么去看，我们做的一些配置呢？

	$ git config --list

这个命令查看的是当前配置, 在当前项目下面查看的配置是全局配置+当前项目的配置, 使用的时候会优先使用当前项目的配置。


##如何操作？

### 一：创建版本库
####版本库是什么？
>版本库又名仓库，也就是我们常见的repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。是不是很屌？
####创建版本库
**上文说到他很屌，但是创建起来确实非常的简单！**

我们只需要选择一个适合的地方，创建一个空目录即可！我们来举个栗子！其实都是一样的！不过这里注意一下，尽量不要用中文命名！就算为了我们的逼格也千万别用！

	$ mkdir www/git/myStudy -p
	$ cd www/git/myStudy
	$ pwd
	/d/www/git/myStudy

下面我们将要用到一个很牛的命令瞬间把这块地方变成你的仓库！

	$ git init

如果你看到出现这种提示，证明你成功了

	Initialized empty Git repository in /root/git/my-study/.git/
这时候你当前目录下会多了一个.git的目录，这个目录是Git来跟踪管理版本的，没事千万不要手动乱改这个目录里面的文件，否则，就会把git仓库给破坏了！你可以用这个命令来查看一下。

	$ ls -ah
####把文件添加到版本库
**接下来简单说几点需要注意的**

1.	所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。
2.	Word格式是二进制格式，因此，版本控制系统是没法跟踪Word文件的改动的，前面我们举的例子只是为了演示，如果要真正使用版本控制系统，就要以纯文本方式编写文件。
3.	千万不要使用Windows自带的记事本编辑任何文本文件。原因是Microsoft开发记事本的团队使用了一个非常弱智的行为来保存UTF-8编码的文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”，明明正确的程序一编译就报语法错误，等等，都是由记事本的弱智行为带来的。

下面我们继续，首先我们在当前目录下来创建一个文件，代码如下

	$ touch readme.md
	$ vi readme.md
我们随便在里面写一些东西进去然后保存

	we are family

那么我们怎么去把这个文件放到仓库里面呢？

1	用命令git add告诉Git，把文件添加到仓库：
	
	$ git add readme.txt

2	用命令git commit告诉Git，把文件提交到仓库：

	$ git commit -m "wrote a readme file"
	[master (root-commit) cb926e7] wrote a readme file
	1 file changed, 1 insertions(+)
	 create mode 100644 readme.txt
上面的意思其实就是，1个文件被改动（我们新添加的readme.txt文件），插入了一行内容（we are family）。**-m "wrote a readme file"**其实就是加了注释，这是作为一个程序员的基本准则！**切记！切记！**

为什么Git添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件，比如：

	$ git add file1.txt
	$ git add file2.txt file3.txt
	$ git commit -m "add 3 files."

我们也可以不用add，当我们提交一个项目，或者文件比较多，基本没问题的前提下可以用下面的代码：

	$ git commit -am "add all"

### 二：git 常用的命令
当我们运行git时会出现这样的代码

	 add        Add file contents to the index
     bisect     Find by binary search the change that introduced a bug
     branch     List, create, or delete branches
     checkout   Checkout a branch or paths to the working tree
     clone      Clone a repository into a new directory
     commit     Record changes to the repository
     diff       Show changes between commits, commit and working tree, etc
     fetch      Download objects and refs from another repository
     grep       Print lines matching a pattern
     init       Create an empty git repository or reinitialize an existing one
     log        Show commit logs
     merge      Join two or more development histories together
     mv         Move or rename a file, a directory, or a symlink
     pull       Fetch from and merge with another repository or a local branch
     push       Update remote refs along with associated objects
     rebase     Forward-port local commits to the updated upstream head
     reset      Reset current HEAD to the specified state
     rm         Remove files from the working tree and from the index
     show       Show various types of objects
     status     Show the working tree status
     tag        Create, list, delete or verify a tag 

**其实这些都是 git的命令 下面我们就来说一说我们常用的命令**


-	####git status命令可以让我们时刻掌握仓库当前的状态
	
		$ git status
		# On branch master
		# Changes not staged for commit:
		#   (use "git add <file>..." to update what will be committed)
		#   (use "git checkout -- <file>..." to discard changes in working directory)
		#
		#    modified:   readme.txt
		#
		no changes added to commit (use "git add" and/or "git commit -a")


-	####git diff查看差异，显示的格式正是Unix通用的diff格式,如果加上具体某个文件名，就可以具体对比某个文件了！

		diff --git a/Learning-growth/git/test.md b/Learning-growth/git/test.md
		index 4dad973..ec71f80 100644
		--- a/Learning-growth/git/test.md
		+++ b/Learning-growth/git/test.md
		@@ -1 +1 @@
		- hello word!
		+hello word!
		diff --git a/readme.md b/readme.md
		index 55bef5e..f4370ba 100644
		--- a/readme.md
		+++ b/readme.md
		@@ -1,2 +1,2 @@
		- we are familu!
		+we are familu!
	　

-	####可以告诉我们历史记录，在Git中，我们用git log命令查看：
		
		
		$ git log
		commit b8d7cbd8d3b5585e495fdf8f0f79a13e8e106fb7
		Author: cuijiji <624337224@qq.com>
		Date:   Wed Feb 3 18:07:28 2016 +0800
		add all

		commit 57b37b7e743b0790213d97fb55d0664c2b18794c
		Author: cuijiji <624337224@qq.com>
		Date:   Wed Feb 3 18:05:25 2016 +0800

    	add a folder

		commit 66d1757cfcc8d1476a23387b133125dc0efb2b96
		Author: cuijiji <624337224@qq.com>
		Date:   Wed Feb 3 17:49:55 2016 +0800

    	add a new file

	如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：

		$ git log --pretty=oneline
		
		b8d7cbd8d3b5585e495fdf8f0f79a13e8e106fb7 add all
		57b37b7e743b0790213d97fb55d0664c2b18794c add a folder
		66d1757cfcc8d1476a23387b133125dc0efb2b96 add a new file

-	####	我们要把当前版本回退到上一个版本就可以使用git reset命令：
		$ git reset --hard HEAD^
		HEAD is now at ea34578 add distributed

	^代表回退上N个版本所以^^代表两个不过如果太往前的我们就会用commit id(这个ID 不用写全 只要前几位保证没有重复即可)

		$ git reset --hard 3628164
		HEAD is now at 3628164 append GPL
### 三：git的工作区和暂存区的概念

####简单以下几点

+	工作区及我们本地仓库的区域，
+	暂存区及我们add上去的区域

####那么我们commit到了哪里了呢？

>我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

####那我们接下来说几个与这个有关的命令吧
**git checkout -- file可以丢弃工作区的修改,也就是让这个文件回到最近一次git commit或git add时的状态：**
		
	$ git checkout -- readme.txt
**git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区**

	$ git reset HEAD readme.txt
	Unstaged changes after reset:
	M       readme.txt
**rm可以删除工作区的文件这时候你可以有两种方式来处理**

第一种从版本库中删除该文件

	$ git rm test.txt
	rm 'test.txt'
	$ git commit -m "remove test.txt"
	[master d17efd8] remove test.txt
	1 file changed, 1 deletion(-)
	delete mode 100644 test.txt
另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本

	$ git checkout -- test.txt


### 四：远程仓库

####什么是远程仓库？
>Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上。怎么分布呢？最早，肯定只有一台机器有一个原始版本库，此后，别的机器可以“克隆”这个原始版本库，而且每台机器的版本库其实都是一样的，并没有主次之分。

####那我们怎么办呢？

>完全可以自己搭建一台运行Git的服务器，不过现阶段，为了学Git先搭个服务器绝对是小题大作。好在这个世界上有个叫GitHub的神奇的网站，从名字就可以看出，这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。

####下面我们来说说具体怎么去做吧

1. 注册GitHub账号，不会的去找教程，我这里不说了！
2. 创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key。如果一切顺利的话，可以在用户主目录里找到.ssh目录，获取id_rsa.pub里面的内容
3. 登陆GitHub，打开“Account settings”，“SSH Keys”页面：
然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：点“Add Key”，你就应该看到已经添加的Key：

**注意：GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。**



####与仓库链接

1.	登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库
2.	在Repository name填入learngit，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：
3.	鉴于我们大多数都是以clone为主，我们就使用下面的代码运行

	git clone git@github.com:cuijiji/study-and-grow-up.git

    这样你就把你的远程仓库克隆下来了！现在我们重复之前的命令来修改，或者添加一个文件
4. 现在我们用下面的命令来提到我们的github上面吧

	git push -u origin master

**注意** 你可能会碰到权限问题，换种链接方式试试！或者联系我吧！


### 五：创建与合并分支

####什么是分支？
>分支就好比数学中平行的几条线！他们之间无不影响，都在自己的世界工作！

####到底是什么？举个栗子说明！
>#####这个问题问的好（自问自答）！但是首先我们要再来理解一下这个概念。经过上面的学习或许你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即master分支。HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。这样说你是否清楚了呢？不清楚没办法！有招想去，没招联系我！

####来创建dev分支，然后切换到dev分支上

**使用下面的命令来创建一个分支dev**
	 
	$ git checkout -b dev

有没有想起来我们之前好像用过这个checkout命令呢？好吧，想不起来算了！那我们重点说下这个命令的意思，其实它是涵盖这一下两个命令：

	$ git branch dev

	$ git checkout dev
那么我们现在试试这个git branch命令的效果吧

	$ git branch
	 * dev
	 master

**git branch**会列出所有的分支，前面带*的分支就是我们当前工作的分支！那我们怎么去切换分支呢？很简单：

	$ git checkout master
	Switched to branch 'master'
现在我们切回dev分支修改一下自己做个测试，然后提交。再切回master，看看你刚才更改的文件是否有跟着变动呢？


>答案是**没有**。**为什么？**
>>这个问题问的真傻？不是说了吗平行，平行的……

###合并分支

**合并分支其实很简单，使用下面这个命令即可：**

	$ git merge dev
	Updating d17efd8..fec145a
	Fast-forward
	 readme.txt |    1 +
	 1 file changed, 1 insertion(+)

屌不屌？我就想问一句，**还有谁？**这里我们对这个命令说明一下，**git merge命令用于合并指定分支到当前分支**所以保证要合并到的是当前分支，也要注意你要合并的分支！**切记切记！**

>没参数的情况下merge是**fast-forward**的，即Git将master分支的指针直接移到dev的最前方。换句话说，如果顺着一个分支走下去可以到达另一个分支的话，那么Git在合并两者时，只会简单移动指针，所以这种合并成为快进式

以上的那种我们称之为直接合并！我们还可以试试这种合并：

>将一条分支上的若干个提交条目压合成一个提交条目，提交到另一条分支的末梢。把dev分支上的所有提交压合成主分支上的一个提交，即压合提交：

　	

	$ git checkout master
	$ git merge --squash dev


>此时，dev上的所有提交已经合并到当前工作区并暂存，但还没有作为一个提交，可以像其他提交一样，把这个改动提交到版本库中：

　　
	$ git commit –m “something from dev”

拣选另一条分支上的某个提交条目的改动带到当前分支上。
每一次提交都会产生一个全局唯一的提交名称，利用这个名称就可以进行拣选提交。
比如在dev上的某个提交叫：321d76f***，我们现在把它合并到master

	$ git checkout master
	$ git cherry-pick 321d76f
>要拣选多个提交，可以给git cherry-pick命令传递-n选项，比如：

　　 
	$ git cherry-pick –n 321d76f

>这样在拣选了这个改动之后，进行暂存而不立即提交，接着可以进行下一个拣选操作，一旦拣选完需要的各个提交，就可以一并提交。

###删除分支
像我这么正直的人，爱做的就是卸磨杀驴！用完了怎么删除？试试这个：

	$ git branch -d dev
	Deleted branch dev (was fec145a).
这样就删除了，其实我们是为了不让自己混乱，因为分支一多就蒙圈了！



### 六：解决冲突

####为什么拿出这个重点说下，因为版本控制器，最蛋疼的问题就是冲突！所以我来告诉大家怎么去解决

首先我们想要解决冲突，就制造个冲突出来。很简单，还记得我们的分支吗？对了，就是这个！我就不在这里重复了，直接来命令

	  $ git checkout -b dev
	  $  git checkout dev
	  $  vim README.md 
	  $  git add README.md 
	  $  git commit -m 'test test'
	  $  git branch
	  $  git branch master
	  $  git checkout master
	  $  vim README.md 
	  $  git add README.md 
	  $  git commit -m 'test master'
	  $  git merge dev

如果你看到的是这样的提示

		Auto-merging README.md
		CONFLICT (content): Merge conflict in README.md
		Automatic merge failed; fix conflicts and then commit the result.

那恭喜你，你冲突了！怎么办？解决！你可以用之前我们用过的命令看一下状态：

		$ git status

你也可以看到哪个文件冲突了！那么我们现在打开刚刚冲突的文件来看一看！

		<<<<<<< HEAD
		## 666
		=======
		## 777
		>>>>>>> dev
>解释一下：<<<<<<<标记冲突开始，后面跟的是当前分支中的内容。

　　HEAD指向当前分支末梢的提交。

　　=======之后，>>>>>>>之前是要merge过来的另一条分支上的代码。

　　>>>>>>>之后的dev是该分支的名字。

**对于简单的合并，手工编辑，然后去掉这些标记，最后像往常的提交一样先add再commit即可。我们可以通过下面这个命令来具体看一下**

	$ git log --graph --pretty=oneline --abbrev-commit

你会看见类似这样的提示：

	*   10fcd7a modify README.md
	|\  
	| * f9b6554 test test
	* | 7d60f76 test master
	|/  

有没有很好懂，但是那个git log 后面跟的一长串 都不记不住。怎么办？下面我来告诉你！怎么把命令简化！

	$ git config --global alias.lg "log --graph --pretty=oneline --abbrev-commit"

接下来你在运行一下 

	$ git lg 

看到了什么？神奇吧！不解释！按照这个写法，你自己去定制几个试试吧！

