# 文件操作
## 改变 Linux 系统文件或目录的访问权限
chmod
https://blog.csdn.net/u013482618/article/details/46473685

## 搜索
Linux的五个查找命令
http://www.ruanyifeng.com/blog/2009/10/5_ways_to_search_for_files_using_the_terminal.html

find、whereis、which 注意下

## 删除文件
使用rm命令式最好带 -i 选项在删除前进行确认
rm 

    rm -i bashrc
    删除文件 bashrc

    rm -i bashrc*
    删除所有文件名以 bashrc 开头的文件

    rm -r /tmp/etc
    删除目录 /tmp/etc

# 软件管理
rpm文件可以理解成windows的安装包，.rpm 文件里面包含编译好的程序，需要的安装环境及这个程序依赖的一些程序，在进行安装时，rpm会先检查安装环境，依赖，只有在这些都没问题时才会安装。

rpm 存在的一个最大的问题是，如果要安装的软件依赖的一些软件没有安装，就无法安装该软件。

yum 就是在 rpm 的基础上解决了这个问题，本质上 yum 还是使用 rpm 安装软件，但是yum在检查到当前要安装的软件依赖的软件没有安装时，yum 会帮我们安装，这样就免去了手动安装的这个流程。

# 系统服务 daemon
从 Centos7.x 开始，传统的 init 已经被舍弃了，取而代之的是 systemd

systemd 向下兼容旧有的 init 服务脚本