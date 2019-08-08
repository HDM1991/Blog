总结来讲，使用 limit 语句，如果不加 order by，不管 offset 的值和limit的值为多少，都会扫描整个表。比如下面这条 sql 语句，即使 offset 等于0，limit 等于1，也需要扫描整个表。如果表的记录数量非常大，速度可想而知。

    SELECT `questions_tgbd`.`id`, `questions_tgbd`.`content`, `questions_tgbd`.`source_data`
    FROM `questions_tgbd`  limit 0,1


如果加了 order by，会扫描 offset + limit 数量的记录。比如下面这条 sql 语句

    SELECT `questions_tgbd`.`id`, `questions_tgbd`.`content`, `questions_tgbd`.`source_data`
    FROM `questions_tgbd` order by id limit 100,1

会扫描 100 + 1 即101条记录。这样有进步，但是如果我想取 order by 排序之后的最后一条记录，等于说还是要扫描整个表。

然后，在扫描过程中，mysql 会把所有记录select字段的值放到扫描集中，最后只取需要的记录。如果 select 的字段很多，可想而知对 sql 的速度影响有多大。


所以，怎么继续优化呢？

    SELECT `questions_tgbd`.`id`, `questions_tgbd`.`content`, `questions_tgbd`.`source_data`
    FROM `questions_tgbd`
    INNER JOIN (SELECT `id`
    FROM `questions_tgbd`
    LIMIT 100
    OFFSET 100000) AS `tmp_0` USING (`id`)

根据实际测试，速度确实快乐，但是为什么会快，解释不清。


[1]:  https://zhuanlan.zhihu.com/p/29090098 "mysql优化之limit优化"
[2]: https://www.jianshu.com/p/efecd0b66c55 "mysql limit 性能优化"
[3]: http://ourmysql.com/archives/1451 "MYSQL分页limit速度太慢优化方法"
[4]: https://blog.csdn.net/fdipzone/article/details/72793837 "mysql查询时，offset过大影响性能的原因与优化方法"