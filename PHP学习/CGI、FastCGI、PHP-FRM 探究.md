# CGI
[CGI][1] 是一个协议。
CGI描述了服务器和请求处理程序之间传输数据的一种标准。

CGI 的工作方式，从Web服务器的角度看，是在特定的位置（比如：*http://www.example.com/wiki.cgi*）定义了可以运行CGI程序。当收到一个匹配URL的请求，相应的程序就会被调用，并将客户端发送的数据作为输入。程序的输出会由Web服务器收集，并加上合适的档头，再发送回客户端。

# FastCGI
[快速通用网关接口][2]（Fast Common Gateway Interface／FastCGI）是一种让交互程序与Web服务器通信的协议。FastCGI是早期通用网关接口（CGI）的增强版本。

*FastCGI致力于减少网页服务器与CGI程序之间交互的开销*，从而使服务器可以同时处理更多的网页请求。

# PHP-FPM
[FPM][3]（FastCGI 进程管理器）用于替换 PHP FastCGI 的大部分附加功能.

[1]: https://zh.wikipedia.org/wiki/%E9%80%9A%E7%94%A8%E7%BD%91%E5%85%B3%E6%8E%A5%E5%8F%A3 "CGI"
[2]: https://zh.wikipedia.org/wiki/FastCGI "FastCGI"
[3]: http://php.net/manual/zh/install.fpm.php "FastCGI 进程管理器（FPM）"
[4]: https://www.awaimai.com/371.html#FastCGI "CGI、FastCGI和PHP-FPM关系图解"
