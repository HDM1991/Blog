# Composer
Composer 将这样为你解决问题：

a) 你有一个项目依赖于若干个库。

b) 其中一些库依赖于其他库。

c) 你声明你所依赖的东西。

d) Composer 会找出哪个版本的包需要安装，并安装它们（将它们下载到你的项目中）。

## 常用命令
# dump-autoload
dump-autoload   Dumps the autoloader.
dumpautoload    Dumps the autoloader.

### remove

### 
### init
以交互式的方式创建一个 composer.json。

### install
install 命令从当前目录读取 composer.json 文件，处理了依赖关系，并把其安装到 vendor 目录下。

如果当前目录下存在 composer.lock 文件，它会从此文件读取依赖版本，而不是根据 composer.json 文件去获取依赖。

### require
require 命令增加新的依赖包到当前目录的 composer.json 文件中，然后安装这个包。

具体用法如下两种：
#### 用法一：
        composer require

上述命令会提供一个交互式的方式来指定包的名字。
#### 用法二    
    composer require vendor/package2

直接在命令行中指定包的名字。

### search
搜索包。
### update
为了获取依赖的最新版本，并且升级 composer.lock 文件。

通俗来讲，就会将包更新到最新的版本。
### show
列出已经安装的包，相当于 bower 的 list 命令。

### global
global 命令允许你在 COMPOSER_HOME 目录下执行其它命令，像 install、require 或 update。

### create-project

    composer create-project --prefer-dist laravel/laravel blog

上述命令，先用 git clone laravel/laravel，然后将目录修改的成 blog，在执行 composer install 命令，安装  laravel/laravel 的依赖。

# 基本流程
以后开始一个新项目时，可以完全通过命令新建建一个 composer.json；或者事先准备一个通用的 composer.json，将其复制到新项目的根目录下，在根据情况进行修改就可以了。

# 私有仓库
有些情况下，我们需要使用私有仓库的php组件（比如公司内部开发的php组件），然后依然可以使用 composer，composer 在安装的时候回询问认证信息。

或者，通过如下命令提前配置认证信息

    composer config http-basic.example.org your-username your-password

# 创建PHP组件
这个暂时不了解了。没这个需求。


# 中国全量镜像
国外网站存在连接慢的问题。
https://pkg.phpcomposer.com/

# 参见问题
1. 如果内存过小也会到之后 composer install 失败。
2. 解决Windows下网络原因Composer安装失败问题
    https://www.cnblogs.com/GaZeon/p/5788712.html

    
[1]: http://docs.phpcomposer.com/00-intro.html "Composer 简介"

