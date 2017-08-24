# 项目设置
尽量保证项目属性页面中，各个设置和 cef 官方提供的示例中的设置相同。这样可以尽可能的避免一些错误。

比如，在这次实验中，刚开始，我没有将链接器中的启用大地址选项设置为是，就出现了程序不能运行的问题。

# 加载资源中的 HTML 文件
在 message_router 中，我们访问的页面的 url 为

    https://example.com/message_router.html

但我们不能让程序真的去访问这个 URL，而是要他访问的资源中 message_router.html，所以需要重载 CefRequestHandler::GetResourceHandler 这个函数进行处理。

## route aynchronous messages betweenJavaScript running in the renderer process and C++ running in the browser process
基于 MFC 的 CDHtmlDialog 开发应用程序时，它提供了同 MFC 原生 UI 开发一样的事件机制和数据绑定技术。但 CEF 并没有这种原生支持，所以这是我们需要解决的一个重大问题。

目前的解决方案就是使用 CEF 提供的 Generic Message Router 支持，虽然它不如 CDHtmlDialog 那么方便，但我们可以很轻松的基于它开发出 CDHtmlDialog 提供的两项支持。但具体怎么弄，还没往深处想。


# 基于 CEF 开发应用的基本架构
基本架构方面，cefclient 这个 sample 是一个很好的例子，我们可以基于这个例子先开发一个小 demo 试下，练练手



