通常我们会遇到这样的情况，将一台电脑上 mysql 数据库中数据以 sql 文件的形式导出，然后再在另外一台电脑上，将这个 sql 文件中的数据导入到新的 mysql 数据库中。

这种情况下通常我们会采用如下两种做法。推荐使用 mysql source 命令。

## 使用 phpmyadmin
phpmyadmin 给我们提供了一个友好的 UI 接口，但是这里并不建议使用这种方法。因为这种方法可能会出现各种各样的问题，比如当sql文件的体积太大时，需要你去修改 php.ini 的如下选项：

* upload_max_filesize
* post_max_size
* memory_limit

然而，即使你修改了上述选项，依然还是可能出错，比如下面这个错误：

> 脚本超时，如果您要完成导入，请重新提交相同的文件，导入会继续进行

上述错误，好像一方面是跟 php.ini 中的下述选项有关，另一方面是跟 phpmyadmin 的 config.inc.php 中 $cfg['ExecTimeLimit'] 的值有关。

* max_execution_time
* max_input_time

但上述两个 php.ini 选项代表的意义和 $cfg['ExecTimeLimit'] 代表的意义，目前我并不是很清楚，目前觉得也没有必要花时间去搞清楚，因为我们推荐使用如下方法。

## mysql source 命令
使用 source 命令，当出现错误时，通常只需要修改 my.ini 中的 max_allowed_packet 选项即可解决问题。如果修改了 max_allowed_packet 依旧出错，那问题可能的原因可能就不是 sql 文件太大了。

[B.5.2.9 Packet Too Large][B.5.2.9 Packet Too Large]中详述了关于该选项的相关信息，我们需要搞清楚如下点：

1. 什么是 Packet
    A communication packet is a single SQL statement sent to the MySQL server, a single row that is sent to the client, or a binary log event sent from a master replication server to a slave.
2. max_allowed_packet 是什么
    max_allowed_packet 就是 MySQL client 或者 mysqld server 能够接收的 packet 的最大的大小。

    注意 MySQL client 和 mysqld server 都是有 max_allowed_packet 选项的。

    When a MySQL client or the mysqld server receives a packet bigger than max_allowed_packet bytes, it issues an ER_NET_PACKET_TOO_LARGE error and closes the connection. With some clients, you may also get a Lost connection to MySQL server during query error if the communication packet is too large.

然后，我们是可以放心的将 max_allowed_packet 设置为一个比较大的值的。

It is safe to increase the value of this variable because the extra memory is allocated only when needed.



[phpmyadmin修改最大限制为20M并且导入sql脚本超时的解决方案]: https://sg.godaddy.com/zh/help/sql-mysql-6802
[B.5.2.9 Packet Too Large]: https://dev.mysql.com/doc/refman/5.7/en/packet-too-large.html
