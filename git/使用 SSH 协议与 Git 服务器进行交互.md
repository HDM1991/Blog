# 如何使用 SSH 协议与 Git 服务器进行交互？
假设我们的开发环境为 Windows 10，项目仓库的地址为 git@code.aliyun.com:HDM1991/demo.git。下面就从头一步一步完成这个需求。

## 第一步，安装 Git
从如下地址下载安装包，直接安装即可。安装后，会有一个 Git Bash，后续的所有命令行操作都在这个 Shell(Git Bash) 中完成。

    安装包下载地址： https://git-for-windows.github.io/

## 第二步，创建 ssh keys
打开 Git Bash，根据[《Create your ssh keys》][1]创建 ssh keys。

这一步存在很多坑。首先，在通过如下命令创建新的 ssh keys 时，

    ssh-keygen -t rsa -C "lionxyes@gmail.com"

ssh-keygen 会显示如下信息：

    $ ssh-keygen -t rsa -C "lionxyes@gmail.com"
    Generating public/private rsa key pair.
    Enter file in which to save the key (/c/Users/HDM/.ssh/id_rsa):

询问在哪里保存生成的 ssh keys。如果直接回车，就使用括号中提供的位置和文件名，这里建议这样做。Why？

因为，括号中的位置和文件名是 OpenSSH 使用的默认值。假如，我们将生成的 ssh keys 放到其他位置，或者说使用了一个非默认的文件名，在不对 OpenSSH 进行配置的情况下，OpenSSH 是不知道如何在哪里寻找我们创建的 ssh keys 的。

那如果有这个需求怎么办，比如我将我生成的 ssh keys 的文件名设置为了 hdm1991_rsa。这时我们就需要对 OpenSSH 进行配置，OpenSSH 的默认配置文件为 ~/.ssh/config，下面是我的 config 文件配置：

> IdentityFile ~/.ssh/hdm1991_rsa

比较简单只有一行。

更加复杂的配置方法，可以参考[《说说Git的ssh key》][2]，其中描述了一些较复杂的使用 SSH 与 Git 服务器进行交互的场景的 config 文件配置方法，比如为每个 Git 服务器指定不同的 ssh keys。

## 第三步，将公钥添加到个人设置中的SSH/My SSH Keys
这里是以，阿里云 code 为例，如果项目仓库放在 Github 上，或者公司的内部 Git 服务器上，步骤稍有不同。


# OpenSSH
其实，使用 SSH 协议与 Git 服务器进行交互的重点就是 OpenSSH 配置，如何让 OpenSSH 找到正确的私钥。通过修改 OpenSSH 的配置文件 config 是一种方法；另外一种方法是使用 ssh-agent，这里就不了解了。

# 其他
SSH 的客户端不止有一种实现，这里我们使用的是 OpenSSH。某些情况下，使用的 SSH 客户端不正确，也会引起问题，如[《Config ssh for windows》][5]中提到的这种情况。

[1]: https://code.aliyun.com/help/code-basics/create-your-ssh-keys.md "Create your ssh keys"
[2]: https://appkfz.com/2015/06/18/git-ssh-key/ "说说Git的ssh key"
[3]: http://blog.csdn.net/shuyong1999/article/details/7566161 "git ssh远程登录"
[4]: https://segmentfault.com/a/1190000002645623 "git-ssh 配置和使用"
[5]: https://code.aliyun.com/help/code-basics/config-ssh-for-windows.md "Config ssh for windows"
[6]: http://blog.csdn.net/newjueqi/article/details/47293897 "利用 ssh 的用户配置文件 config 管理 ssh 会话"