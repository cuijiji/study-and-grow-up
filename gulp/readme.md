---
title: gulp
date: 2016-03-07 11:51:53
tags:
---


# 简介 #

gulp是前端开发过程中对代码进行构建的工具，是自动化项目的构建利器；她不仅能对网站资源进行优化，而且在开发过程中很多重复的任务能够使用正确的工具自动完成；使用她，我们不仅可以很愉快的编写代码，而且大大提高我们的工作效率。

# gulp能帮我们做什么? #

+ javascript语法的检测、合并，压缩。
+ css的压缩、合并、sass/less的编译
+ images的压缩、合并
+ 浏览器自动刷新
+ 部署文件生产替换
+ 监控文件变化并自动生产相应文件
 
# gulp的安装 #

gulp是基于Nodejs的自动任务运行器，这里默认已经安装了nodejs

> 全局安装gulp

    npm install --global gulp

> 作为项目的开发依赖安装

    npm install --save-dev gulp

> 在项目根目录下创建名为gulpfile.js的文件

    var gulp = require('gulp');

    gulp.task('default', function(){});

> 运行gulp 安装完成

    gulp -v

    gulp

## 正常安装完成会出现版本号以及执行默认任务 ##

> 依赖文件

 别忘记在项目的根目录下还应该建相应的资源依赖文件package.json文件，它只是一个普通的json文件(但是不可以写注释)，但是对项目来说，不可或缺，里面不仅存在项目的具体描述信息，还存有项目所需要的包依赖，使用它可以记录相关的gulp插件，以便多人开始时协同使用，另外也可以指令建立：

	npm init

# gulp原理 #

  在实现上，glup借鉴了Unix操作系统的管道（pipe）思想，前一级的输出，直接变成后一级的输入，使得在操作上非常简单。但是需要注意的是，gulp运行的过程中不会产生临时文件，所有的文件都是虚拟的，直到指定相应目录进行输出。

# gulp函数 #

## gulp.src() ##

gulp.src()方法输入一个glob(比如匹配一个或多个文件的字符串)或者glob数组，然后返回一个可以传递给插件的数据流。

Gulp使用node-glob来从你指定的glob里面获取文件,主要匹配规则有以下规则（以js为例）

+ js/app.js 精确匹配文件
+ js/*.js 仅匹配js目录下的所有后缀为.js的文件
+ js/*/.js 匹配js目录及其子目录下所有后缀为.js的文件
+ !js/app.js 从匹配结果中排除js/app.js，这种方法在你想要匹配除了特殊文件之外的所有文件时非常管用
+ *.+(js|css) 匹配根目录下所有后缀为.js或者.css的文件

## gulp.task() ##

gulp.task()函数通常会被用来定义任务。当你定义一个简单的任务时，需要传入任务名字和执行函数两个属性。

    gulp.task('greet', function () {
   		console.log('Hello world!');
	});
一个任务有时也可以是一系列任务。假设要定义一个任务build来执行css、js、imgs这三个任务，我们可以通过指定一个任务数组而不是函数来完成。

	gulp.task('build', ['css', 'js', 'imgs']);

这些任务不是同时进行的，所以你不能认为在js任务开始的时候css任务已经结束了，也可能还没有结束。为了确保一个任务在另一个任务执行前已经结束，可以将函数和任务数组结合起来指定其依赖关系。例如，定义一个css任务，在执行前需要检查greet任务是否已经执行完毕，这样做就是可行的:

	gulp.task('css', ['greet'], function () { });

现在，当执行css任务时，Gulp会先执行greet任务，然后在它结束后再调用你定义的函数。

你可以定义一个在gulp开始运行时候默认执行的任务，并将这个任务命名为“default”：

	gulp.task('default', function () { });

## gulp.dest() ##

gulp.dest()方法是用来写文件的，他将经过gulp处理的文件输出到相应目录

	var gulp = reruire('gulp');
	gulp.src('script/**/*.js')
    	.pipe(gulp.dest('dist'));

## gulp.watch() ##

gulp.watch()用来监视文件的变化，当文件发生变化后，我们可以利用它来执行相应的任务，例如文件压缩等

	gulp.watch(glob[, opts], tasks)

Gulp可以监听文件的修改动态，然后在文件被改动的时候执行一个或多个任务。这个特性十分有用（对我来说，这可能是Gulp中最有用的一个功能）。你可以保存LESS文件，接着Gulp会自动把它转换为CSS文件并更新浏览器。
	
使用gulp.watch()方法可以监听文件，它接受一个glob或者glob数组（和gulp.src()一样）以及一个任务数组来执行回调。

让我们看看下面，build任务可以将模板转换成html格式，然后我们希望定义一个watch任务来监听模板文件的变化，并将这些模板转换成html格式。watch函数的使用方法如下所示：

	gulp.task('watch', function () {
   		gulp.watch('templates/*.tmpl.html', ['build']);
	});

现在，当改变一个模板文件时，build任务会被执行并生成HTML文件，也可以给watch函数一个回调函数，而不是一个任务数组。在这个示例中，回调函数有一个包含触发回调函数信息的event对象：

	gulp.watch('templates/*.tmpl.html', function (event) {
   		console.log('Event type: ' + event.type); // added, changed, or deleted
   		console.log('Event path: ' + event.path); // The path of the modified file
	});

Gulp.watch()的另一个非常好的特性是返回我们熟知的watcher。利用watcher来监听额外的事件或者向watch中添加文件。例如，在执行一系列任务和调用一个函数时，你就可以在返回的watcher中添加监听change事件:

	var watcher = gulp.watch('templates/*.tmpl.html', ['build']);
		watcher.on('change', function (event) {
   		console.log('Event type: ' + event.type); // added, changed, or deleted
   		console.log('Event path: ' + event.path); // The path of the modified file
	});

除了change事件，还可以监听很多其他的事件:

+ end 在watcher结束时触发（这意味着，在文件改变的时候，任务或者回调不会执行）
+ error 在出现error时触发
+ ready 在文件被找到并正被监听时触发
+ nomatch 在glob没有匹配到任何文件时触发
+ Watcher对象也包含了一些可以调用的方法：

+ watcher.end() 停止watcher（以便停止执行后面的任务或者回调函数）
+ watcher.files() 返回watcher监听的文件列表
+ watcher.add(glob) 将与指定glob相匹配的文件添加到watcher（也接受可选的回调当第二个参数）
+ watcher.remove(filepath) 从watcher中移除个别文件

# gulp常用插件 #

> js语法检测

	var jshint = require("gulp-jshint"),

> js压缩

	var uglify = require("gulp-uglify"),

> css压缩

	var minifycss = require("gulp-minify-css"),

> sass编译

	var sass = require("gulp-ruby-sass"),

> 图片压缩

	var imagemin = require("gulp-imagemin")

> 雪碧图合并

	var spriter = require("gulp-css-spriter")

> js、css的合并

	var concat = require("gulp-concat")

> 文件重命名

	var rename = require("gulp-rename")

> MD5戳

	var md5 = require("gulp-rev")

> 路径替换

	var collector = require("gulp-rev-collector") 

> 刷新浏览器

	var reload = require("gulp-livereload")

以上这些就是gulp常用的插件，至于安装以及其中的用法，这里不做一一介绍，有兴趣的朋友自行研究即可。

# 小结 #

gulp作为前端的开发的自动化构建工具，确实帮我们完成了很多必要的工作，相对于其他前端构建工具运行速度快，一直为gulp所骄傲，但是同时由于项目的复杂性，以及认为的灵活性，gulp有时也会遇到瓶颈，建议可以参考参考其它构建工具，诸于fis等。

（gulp的几大函数基本参数是必须掌握的，其中还有一些不常用的参数配置，这里不一一介绍，在项目中进行锻炼时可以慢慢体会）

下面是gulp的两个参考网站：

+ [gulp中文网](http://www.gulpjs.com.cn/)

+ [gulp系列教程](http://www.w3cplus.com/blog/tags/528.html)
