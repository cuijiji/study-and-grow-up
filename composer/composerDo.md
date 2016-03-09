composer的使用
===


### 声明依赖
>在项目目录下创建一个 composer.json 文件，指明依赖，比如，你的项目依赖 monolog：

    $ vim composer.json

#### 然后在添加以下内容

    {
      "require": {
          "monolog/monolog": "1.2.*"
      }
    }

### 安装依赖

#### 安装依赖非常简单，只需在项目目录下运行：

    $ composer install

#### 如果你报错
     ‘Your configuration does not allow connection to http://packagist.phpcomposer.com. See https://getcomposer.org/doc/06-config.md#secure-http for details’

#### 打开命令行窗口（windows用户）或控制台（Linux、Mac 用户），进入你的项目的根目录（也就是 composer.json 文件所在目录），执行如下命令：

    $ composer config repo.packagist composer https://packagist.phpcomposer.com

#### 上述命令将会在当前项目中的 composer.json 文件的末尾自动添加镜像的配置信息（你也可以自己手工添加）：

    "repositories": {
      "packagist": {
      "type": "composer",
      "url": "https://packagist.phpcomposer.com"
      }
    }
#### composer.json架构

+ 包名 name*描述 description*
+ 版本 version（不是必须的，并且建议忽略）
+ 安装类型 type（library默认，project项目）
+ 关键字 keywords
+ 项目主页 homepage
+ 版本发布时间 time
+ 许可协议 license
+ 作者 authors
+ 支持 support
+ require 依赖包列表
+ require-dev 开发用包
+ autoload 自动加载
    PSR-4、PSR-0、Classmap
+ autoload-dev
+ repositories使用自定义的包资源库
+ config 配置
+ scripts安装过程中的各个阶段挂接脚本
