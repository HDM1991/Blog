# 问题
在 PHP 中什么是 error，什么是 exception?

为什么同一段代码，不使用任何框架，直接放到 PHP 中执行就是 error，放到 laravel 中就可以捕获异常？


在PHP中任何自身的错误或者是非正常的代码都会当做错误对待，并不会以异常的形式抛出。

在 php 中 error 和 exception 是两种概念。

php 并没有统一使用 exception 来处理程序中的错误，而是同时存在 error 和 exception 两种错误处理方式。

很多 php 函数在发生错误时，使用 error 的方式来反馈错误。比如 file_get_contents.

原先的 PHP 只有错误没有异常.

好乱。

# exception
PHP 5 提供了一种新的面向对象的错误处理方法。

# set_error_handler

# set_exception_handler


# 总结
php 中的错误处理同时存在 error 和 exception 的概念，php 并没有统一使用异常来处理错误。

同时存在这两种概念并不好，所以 Laravel 替我们提供了统一使用异常来处理错误的机制。包括 PHP 自身，其实也在往使用异常来统一处理错误这条路上靠。

比如，在 PHP 7 中，一些 error 就变成了可捕获的异常。

[1]: http://php.net/manual/en/function.set-error-handler.php

[2]: https://juejin.im/entry/5987d2ff6fb9a03c314fe732 "php 的错误和异常处理机制"
[3]: http://www.cnblogs.com/zyf-zhaoyafei/p/6928149.html#excetion3 "再谈PHP错误与异常处理"
[4]: http://www.cnblogs.com/zyf-zhaoyafei/p/3649434.html "php的错误级别
"