# http referrer
HTTP 请求的头信息里面，referrer 是一个常见字段，提供访问来源的信息。

这个字段的意义就是让服务器知道打开这个链接的来源是什么？比如是某个网页吗？


一个典型的应用是，有些网站不允许图片外链，只有自家的网站才能显示图片，外部网站加载图片就会报错。它的实现就是基于Referer字段，如果该字段的网址是自家网址，就放行。


# 什么情况下浏览器发送请求时会在请求头中加入 referrer

1. 用户在地址栏输入网址，或者选中浏览器书签，就不发送Referer字段。

主要是以下三种场景，会发送Referer字段。

1. 用户点击网页上的链接。
2. 用户发送表单。
3. 网页加载静态资源，比如加载图片、脚本、样式。

        <!-- 加载图片 -->
        <img src="foo.jpg">
        <!-- 加载脚本 -->
        <script src="foo.js"></script>
        <!-- 加载样式 -->
        <link href="foo.css" rel="stylesheet">


# 改变浏览器默认的Referer行为
## rel属性

<a>、<area>和<form>三个标签可以使用这个属性，一旦使用，该元素就不会发送Referer字段。

    <a href="..." rel="noreferrer" target="_blank">xxx</a>

上面链接点击产生的 HTTP 请求，不会带有Referer字段。

## Referrer Policy 的值
Referrer Policy 可以设定8个值。
1. no-referrer
    不发送Referer字段。
2. no-referrer-when-downgrade
    如果从 HTTPS 网址链接到 HTTP 网址，不发送Referer字段，其他情况发送（包括 HTTP 网址链接到 HTTP 网址）。这是浏览器的默认行为。
3. same-origin
    链接到同源网址（协议+域名+端口 都相同）时发送，否则不发送。注意，https://foo.com链接到http://foo.com也属于跨域。
4. origin
    Referer字段一律只发送源信息（协议+域名+端口），不管是否跨域。
5. strict-origin
    如果从 HTTPS 网址链接到 HTTP 网址，不发送Referer字段，其他情况只发送源信息。
6. origin-when-cross-origin
    同源时，发送完整的Referer字段，跨域时发送源信息。
7. strict-origin-when-cross-origin
    同源时，发送完整的Referer字段；跨域时，如果 HTTPS 网址链接到 HTTP 网址，不发送Referer字段，否则发送源信息。
8. unsafe-url
    Referer字段包含源信息、路径和查询字符串，不包含锚点、用户名和密码。


第三种 same-origin 用来解决有些网站不允许图片外链的问题不错。


### 如何使用Referrer Policy
1. HTTP 头信息
    服务器发送网页的时候，通过 HTTP 头信息的Referrer-Policy告诉浏览器。

        Referrer-Policy: origin
2. <meta>标签
也可以使用<meta>标签，在网页头部设置。

        <meta name="referrer" content="origin">
3. referrerpolicy属性
    <a>、<area>、<img>、<iframe>和<link>标签，可以设置referrerpolicy 属性。

        <a href="..." referrerpolicy="origin" target="_blank">xxx</a>



https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta



referrer  控制所有从该文档发出的 HTTP 请求中HTTP Referer 首部的内容：
<meta name="referrer"> content 属性可取的值：


no-referrer 不要发送 HTTP Referer 首部。
origin  发送当前文档的 origin。
no-referrer-when-downgrade  当目的地是先验安全的(https->https)则发送 origin 作为 referrer ，但是当目的地是较不安全的 (https->http)时则不发送 referrer 。这个是默认的行为。
origin-when-crossorigin 在同源请求下，发送完整的URL (不含查询参数) ，其他情况下则仅发送当前文档的 origin。
unsafe-URL  在同源请求下，发送完整的URL (不含查询参数)。


[1]: http://www.ruanyifeng.com/blog/2019/06/http-referer.html "HTTP Referer 教程"
[2]: https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta "HTML <meta> 元素"