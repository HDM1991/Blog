目前对这个东西的高级点的用法也没搞清楚，但现在掌握的这些已经满足我们的需求了，就先不探索高级用法了。


# xdebug 的安装
以 mac 系统为例，如果 php 是通过 brew 安装的，那 xdebug 也可以通过 brew 安装。

    # brew install homebrew/php/<php-version>-xdebug
eg.

    # brew install homebrew/php/php71-xdebug

The Xdebug extension will be enabled per default after the installation, additional configuration of the extension should be done by adding a custom ini-file to /usr/local/etc/php/<php-version>/conf.d/ 

通过 brew 安装 xdebug 后，xdebug 已经启用了，不用在配置 php.ini。

然后我们要做的就是通过修改 /usr/local/etc/php/7.1/conf.d/ext-xdebug.ini 文件，来配置 xdebug 的一些选项。

在 /usr/local/etc/php/7.1/conf.d/ext-xdebug.ini 中加入如下两项。

xdebug.remote_enable = On
xdebug.idekey = PHPSTORM

# 浏览器安装 Xdebug helper 插件
将 Xdebug helper 的 IDE key 配置项，改为 PhpStorm:PHPSTORM，如下图所示：
![Xdebug helper](http://img.blog.csdn.net/20180106162051240?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzQ4MjYxOA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# 配置 phpstorm
如下图所示，这也是 phpstorm 的默认配置。
![phpstorm xdebug](http://img.blog.csdn.net/20180106162138454?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzQ4MjYxOA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# 开始调试
首先，将 Xdebug helper 设置 Debug 模式，如下图所示：
![这里写图片描述](http://img.blog.csdn.net/20180106170344583?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzQ4MjYxOA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

然后，在 phpstorm 的 Run 菜单下点 Start Listening For PHP Debug Connections 菜单项，如下图所示：
![这里写图片描述](http://img.blog.csdn.net/20180106170712489?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzQ4MjYxOA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

最后，访问 URL。

[1]: https://xdebug.org/docs/install "xdebug install"
[2]: https://confluence.jetbrains.com/display/PhpStorm/Zero-configuration+Web+Application+Debugging+with+Xdebug+and+PhpStorm "Zero-configuration Web Application Debugging with Xdebug and PhpStorm"
[3]: https://segmentfault.com/a/1190000004175313 "phpstorm配置Xdebug进行调试PHP教程"
[4]: https://segmentfault.com/a/1190000011907425 "PHPStorm + Xdebug 配置使用教程"
[5]: https://segmentfault.com/a/1190000008837245 "使用xdebug+phpstorm+(chrome|postman) 调试php"