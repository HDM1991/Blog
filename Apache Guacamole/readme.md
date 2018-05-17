# 什么是 Apache Guacamole

Apache Guacamole is a clientless remote desktop gateway. It supports standard protocols like VNC, RDP, and SSH.

once Guacamole is installed on a server, all you need to access your desktops is a web browser.

简单来说，通过 Apache Guacamole，你可以使用浏览器访问远程桌面，而且不需要安装任何浏览器插件。

# Apache Guacamole 组成

Guacamole is separated into two pieces: guacamole-server, which provides the guacd proxy and related libraries, and guacamole-client, which provides the client to be served by your servlet container, usually Tomcat.

Guacamole 由两部分组成：guacamole-server 和 guacamole-client。

guacamole-client 就是一个 web app，基于 java 和 js 开发。

guacamole-server 的作用是什么，下面通过一张图来说明。

首先，说下传统的远程桌面访问。如下图所示，传统的传统的远程桌面访问就是 client 直接连接 server。client 就是远程桌面客户端，server 就是指开启了远程桌面服务的电脑。

但 Guacamole 不是这样，它是 clientless 的，我们是通过浏览器来访问远程桌面的，所以它是这样的。如下图所示

这里我们通过浏览器访问的其实是 guacamole-client 这个 web app，然后 guacamole-client 并没有直接去连接开启了远程桌面服务的电脑，它连接的是 guacamole-server，然后由 guacamole-server 去连接这个开启了远程桌面服务的电脑。

所以 guacamole-server 可以理解成 guacamole-client 和开启了远程桌面服务的电脑之间的一个代理。

# Apache Guacamole 配置
因为 Apache Guacamole 由两部分组成，两部分各有各的配置。

## guacamole-client 配置
guacamole-client 的配置主要涉及到两个文件：guacamole.properties 和 user-mapping.xml。

guacamole.properties 是 guacamole-client 的核心配置文件，guacamole-client 默认情况下会从当前用户目录下的 .guacamole 目录下读取这个文件。

guacamole.properties 涉及到的配置项很多，大部分都有默认值。这里我们主要关心如下配置项：

    guacd-hostname
    The host the Guacamole proxy daemon (guacd) is listening on. If omitted, Guacamole will assume guacd is listening on localhost.

    guacd-port
    The port the Guacamole proxy daemon (guacd) is listening on. If omitted, Guacamole will assume guacd is listening on port 4822.

    guacd-ssl
    If set to "true", Guacamole will require SSL/TLS encryption between the web application and guacd. By default, communication between the web application and guacd will be unencrypted.

guacd-hostname 和 guacd-port 配置项指定 guacamole-server 的地址，guacd-ssl 配置项指定 guacamole-client 和 guacamole-server 之间的数据交互是否要加密。其他配置项通常使用默认值即可。

guacamole-client 支持多种认证方式，但默认的认证方式就是使用 user-mapping.xml。guacamole-client 默认情况下也是从当前用户目录下的 .guacamole 目录读取 user-mapping.xml 文件。

在 user-mapping.xml 文件中，首先要定义的是用户，并为该用户指定密码。其次，在该用户下定义连接。连接这里指，要访问的远程桌面的信息，比如地址，协议相关的参数，可以为一个用户定义多个连接。

## guacamole-server
guacamole-server 的配置信息存放在 guacd.conf 文件中，该文件位于 /etc/guacamole/ 目录下。在该文件中，主要设置如下配置项。

* bind_host
* bind_port
* server_certificate
* server_key

bind_host 和 bind_port 指定 guacamole-server 要监听的端口。server_certificate 和 server_key 指定用于加密 guacamole-client 和 guacamole-server 通信数据的证书信息，当 guacamole.properties 中的 guacd-ssl 的配置项为 false 时，这两项不指定也可以。

# 下一步要研究的点

1. RDP 协议相关参数的配置
    目前只是对认证信息进行配置，后面可能需要对其他配置项（如分辨率，dpi等）进行配置
2. 虚拟机管理
    这个和 Apache Guacamole 关系不大，但肯定是要研究的。

    如果我们要使用 Apache Guacamole，那具体情形应该是这样的，当用户需要一个做题环境时，虚拟机管理系统就实时创建一个虚拟机，并把这个虚拟机的连接信息反馈给 Apache Guacamole，然后在使用 Apache Guacamole 访问这个虚拟机。

    所以这里涉及到的虚拟机管理应该包含如下点：

    1. 实时的创建和删除
    2. 保存用户的做题情况，并且能够恢复

3. guacamole-client
    如果我们使用 Apache Guacamole，guacamole-client 肯定是不能直接用的，应该是使用 Apache Guacamole 提供的 API，做一套适合我们自己的系统。这个应该不难。

