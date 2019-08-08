针对特定的程序与特定的文件资源来进行权限的控管

安全性本文存在於主体程序中与目标文件资源中。

[root@localhost s2]# whereis php-fpm
php-fpm: /usr/sbin/php-fpm /etc/php-fpm.d /etc/php-fpm.conf /usr/share/man/man8/php-fpm.8.gz
[root@localhost s2]# ll -Zd /usr/sbin/php-fpm
-rwxr-xr-x. root root system_u:object_r:httpd_exec_t:s0 /usr/sbin/php-fpm



    
    [root@www ~]# setenforce [0|1]
    选项与参数：
    0 ：转成 permissive 宽容模式；
    1 ：转成 Enforcing 强制模式

    范例一：将 SELinux 在 Enforcing 与 permissive 之间切换与观察
    [root@www ~]# setenforce 0
    [root@www ~]# getenforce
    Permissive
    [root@www ~]# setenforce 1
    [root@www ~]# getenforce
    Enforcing

    ps aux -Z |grep nginx
    ps aux -Z |grep php-fpm

    system_u:system_r:httpd_t:s0    apache   11535  0.0  0.2 550772 19964 ?        S    10鏈?2   0:00 php-fpm: pool www

    system_u:system_r:cd :s0    root     16380  0.0  0.0 125420  4040 ?        S    10鏈?2   0:00 nginx: worker process


    [root@www ~]# chcon [-R] [-t type] [-u user] [-r role] 文件
    [root@www ~]# chcon [-R] --reference=范例档 文件
    选项与参数：
    -R  ：连同该目录下的次目录也同时修改；
    -t  ：后面接安全性本文的类型栏位！例如 httpd_sys_content_t ；
    -u  ：后面接身份识别，例如 system_u；
    -r  ：后面街角色，例如 system_r；
    --reference=范例档：拿某个文件当范例来修改后续接的文件的类型！

    范例一：将刚刚的 index.html 类型改为 httpd_sys_content_t 的类型
    [root@www ~]# chcon -t httpd_sys_content_t /var/www/html/index.html
    [root@www ~]# ll -Z /var/www/html/index.html
    -rw-r--r--  root root root:object_r:httpd_sys_content_t /var/www/html/index.html
    # 瞧！这样就改回来啦！

    范例二：以 /etc/passwd 为依据，将 index.html 修改成该类型
    [root@www ~]# ll -Z /etc/passwd
    -rw-r--r--  root root system_u:object_r:etc_t          /etc/passwd

    [root@www ~]# chcon --reference=/etc/passwd /var/www/html/index.html
    [root@www ~]# ll -Z /var/www/html/index.html
    -rw-r--r--  root root root:object_r:etc_t /var/www/html/index.html
    # 看看！是否与上面的 /etc/passwd 相同了！不过，这又是错误的安全性本文！
    # 先不要急著修改！我们来进行底下的另外一个命令处置看看！