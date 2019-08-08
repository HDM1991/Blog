其实原生的 Date 对象已经支持我们的大部分操作了，只是我们没发现。


本身 Date 构造函数就支持4种形式。


1. new Date();
2. new Date(value);
3. new Date(dateString);
4. new Date(year, monthIndex [, day [, hours [, minutes [, seconds [, milliseconds]]]]]);


第二种参数是一个 Unix 时间戳（Unix Time Stamp），它是一个整数值，表示自1970年1月1日00:00:00 UTC（the Unix epoch）以来的毫秒数。


然后第三种参数是表示日期的字符串值。该字符串应该能被 Date.parse() 正确方法识别

然后第四种所定义参数代表的是*当地时间*。如果需要使用世界协调时 UTC，使用 new Date(Date.UTC(...)) 和相同参数。




# 从日期字符串创建 Date 对象
本身 Date 的构造函数就支持这种格式，但是官方貌似不推荐。因为标准不一样。

>注意: 由于浏览器之间的差异与不一致性，强烈不推荐使用Date构造函数来解析日期字符串 (或使用与其等价的Date.parse)。对 RFC 2822 格式的日期仅有约定俗称的支持。 对 ISO 8601 格式的支持中，仅有日期的串 (例如 "1970-01-01") 会被处理为 UTC 而不是本地时间，与其他格式的串的处理不同。


# taylorhakes/fecha
上面提到说，不要用Date构造函数直接从日期字符串创建 Date 对象。所以可以使用 fecha 这个库，本身比较小。该有的功能也都有。

    fecha.parse('10-12-10 14:11:12', 'YY-MM-DD HH:mm:ss'); // new Date(2010, 11, 10, 14, 11, 12)
    var date = fecha.parse('2019/7/15', 'YYYY/M/DD'); 


注意 fecha.parse 本质上使用 Date 构造函数的第四种形式创建 Date 对象，所以不存在解析结果可能出现不一致的问题。


这个库以后可以多用用。moment/moment 那个库就算了，太TM大了。

还有date-fns这个库，应该也不错，但也挺大的。



[1]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date "MDN Date"
[2]: https://github.com/taylorhakes/fecha "taylorhakes/fecha"
[3]: https://github.com/date-fns/date-fns "date-fns"