# 环境
mac os 10.13.4 

# 安装 nginx
    
    brew install nginx

下面是我安装的 nginx 的相关信息。

    xxxMacBook-Pro:~ dmh$ brew info nginx
    nginx: stable 1.13.12 (bottled), HEAD
    HTTP(S) server and reverse proxy, and IMAP/POP3 proxy server
    https://nginx.org/
    /usr/local/Cellar/nginx/1.13.12 (23 files, 1.4MB) *
      Poured from bottle on 2018-05-14 at 17:12:42
    From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/nginx.rb
    ==> Dependencies
    Required: openssl ✔, pcre ✔
    Optional: passenger ✘
    ==> Options
    --with-passenger
        Compile with support for Phusion Passenger module
    --HEAD
        Install HEAD version
    ==> Caveats
    Docroot is: /usr/local/var/www

    The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
    nginx can run without sudo.

    nginx will load all files in /usr/local/etc/nginx/servers/.

    To have launchd start nginx now and restart at login:
      brew services start nginx
    Or, if you don't want/need a background service you can just run:
      nginx

首先，nginx 的版本为 1.13.12; 然后需要注意的一些信息是

1. Docroot is: /usr/local/var/www
2. nginx will load all files in /usr/local/etc/nginx/servers/. 也就是说当需要配置一个服务器时，在 /usr/local/etc/nginx/servers/ 目录下创建一个该服务器的配置文件就可以了。
3. 怎么样启动 nginx

# php-fpm
nginx 需要配合 php-fpm 解析 php。

## 以服务的方式启动通过 brew 安装的 php-fpm
    brew services start php@7.1

也可以直接运行，只需要像下面这样
    
    php-fpm

需要注意的一点是，启动 php-fpm 时的使用的账户一定要拥有站点目录和文件的读权限，否则访问页面时就会返回 404 file not found 的错误。

另外一个很蛋疼的点在于使用 mac 自带的 php-fpm 会出现一些列的问题，详细情况可以参考这个文档[《[开发环境]Mac 配置 php-fpm》][1].

但是如果使用通过 brew 安装的 php-fpm 就不会出现这个问题。所以我们要解决的问题就是怎么使用 brew 安装的 php 和 php-fpm. 为了解决这个问题，我们需要做到以下两点。

1. 在 /usr/local/bin 和 /usr/local/sbin 中创建 php 和 php-fpm 的链接。这点可以通过 brew link 命令实现。

2. 在系统搜索 php 和 php-fpm 时，先搜索 /usr/local/bin 和 /usr/local/sbin 目录，在搜索 /usr/bin 和 /usr/sbin 目录; 这点我们可以通过编辑 /etc/paths 文件，将 /usr/local/bin 和 /usr/local/sbin 放到 /usr/bin 和 /usr/sbin 前面就可以了。


# brew link

    DMdeMacBook-Pro:~ dmh$ brew help link
    brew ln, link [--overwrite] [--dry-run] [--force] formula:
        Symlink all of formula's installed files into the Homebrew prefix. This
        is done automatically when you install formulae but can be useful for DIY
        installations.

        If --overwrite is passed, Homebrew will delete files which already exist in
        the prefix while linking.

        If --dry-run or -n is passed, Homebrew will list all files which would
        be linked or which would be deleted by brew link --overwrite, but will not
        actually link or delete any files.

        If --force (or -f) is passed, Homebrew will allow keg-only formulae to be linked.

注意上面上面这段话中几个单词的意思。formula、Homebrew prefix、DIY
        installations、keg-only formulae。

formula 应该就是能通过 brew 安装的程序，每个通过 brew 安装的程序就是一个 formula。

Homebrew prefix 就是指 /usr/local 目录

keg-only formulae - 应该是指安装时没有 link 的 formula。


# 其他

    netstat -aln -p tcp | grep LISTEN



[1]: https://github.com/musicode/test/issues/5 "[开发环境]Mac 配置 php-fpm"



ERROR: No pool defined. at least one pool section must be specified in config file
http://www.cnblogs.com/xiaozong/p/5458169.html


https://blog.csdn.net/lbinin/article/details/70185640


杂谈---mac下nginx+php-fpm
http://www.cnblogs.com/zhongyuan/p/3313106.html


nginx location 配置   