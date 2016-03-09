composer 命令
===
###  初始化
    $ composer init

### 安装

    $ composer install
>如果当前目录下存在 composer.lock 文件，它会从此文件读取依赖版本，而不是根据 composer.json 文件去获取依赖。这确保了该库的每个使用者都能得到相同的依赖版本

### 获取依赖的最新版本并且升级 composer.lock
    $ composer update
###  下载新的依赖包并添加到当前目录的 composer.json 文件中

    $ composer require

### 将 Composer 自身升级到最新版本

    $ composer self-update

### 创建一个新项目
    $ composer create-project


###  查看配置
    $ composer config --list
 ### 全局执行
    $ composer global
### 搜索
    $ composer search
### 列出所有可用的包  （包括未下载的，composer show -i 已下载的）

    $ composer show

### 某个包的详细信息

    $ composer show monolog/monolog
### 依赖性检测

    $ composer depends
### 在发布前检测 composer.json 文件是否是有效的

    $ composer validate

### 依赖包状态检测

    $ composer status
