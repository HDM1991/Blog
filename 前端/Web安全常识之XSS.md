# Cross Site Scripting



# CSRF
[《浅谈CSRF攻击方式》][4]好文章。

关于 CSRF 的防御，本质上就是在客户端请求中添加一个加密过的字符串，然后在服务端将未加密的字符串加密后与客户端提交的加密字符串进行比较，一样，说明请求合法。

因为攻击者不知道未加密的字符串是什么和加密方式，也就无法伪造加密字符串。

具体实现时，可以在所有表单中使用同一字符串，也可以为每一个用户单独构造一个字符串。Laravel 就是为每一个用户单独构造一个字符串。

# 如果处理富文本中 XSS
[HTML Purifier][2]这个有时间了解一下。

[《在 Laravel 5 中使用 Purifier 扩展包集成 HTMLPurifier 防止 XSS 跨站攻击》][3]中讲解了如下在 laravel 中使用 HTML Purifier。


[1]: http://www.cnblogs.com/TankXiao/archive/2012/03/21/2337194.html "Web安全测试之XSS"
[2]: http://htmlpurifier.org/ "HTML Purifier"
[3]: http://laravelacademy.org/post/3914.html "在 Laravel 5 中使用 Purifier 扩展包集成 HTMLPurifier 防止 XSS 跨站攻击"

[4]: https://www.cnblogs.com/hyddd/archive/2009/04/09/1432744.html "浅谈CSRF攻击方式"