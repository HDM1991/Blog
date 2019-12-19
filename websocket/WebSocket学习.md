http://www.ruanyifeng.com/blog/2017/05/websocket.html

其实本质上就是 socket 编程，而且是简化的

（1）建立在 TCP 协议之上，服务器端的实现比较容易。
（2）与 HTTP 协议有着良好的兼容性。默认端口也是80和443，并且握手阶段采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。




# WebSocket 服务器实现
[编写 WebSocket 服务器][1]

WebSocket服务器是一个TCP应用程序。

WebSocket服务器可以用任何实现了Berkeley sockets的服务器端编程语言编写,如C(++)或Python甚至PHP和服务器端JavaScript

## WebSocket 握手
首先，服务器必须使用标准的TCP套接字来监听传入的套接字连接。



# socket.io 
https://socket.io/

Socket.IO is a library that enables real-time, bidirectional and event-based communication between the browser and the server. It consists of:

a Node.js server: Source | API
a Javascript client library for the browser (which can be also run from Node.js): Source | API

## 画板example
很简单，就是客户端将划线的数据发送到服务器，服务器将数据广播到所有页面。页面接到数据划线即可。

## 聊天example
本质上跟画板没啥区别


# workerman
https://github.com/walkor/workerman/
An asynchronous event driven PHP framework. Supports HTTP, Websocket, SSL and other custom protocols


[1]: https://developer.mozilla.org/zh-CN/docs/Web/API/WebSockets_API/Writing_WebSocket_servers "编写 WebSocket 服务器"
[2]: http://www.ruanyifeng.com/blog/2017/05/websocket.html "WebSocket 教程"