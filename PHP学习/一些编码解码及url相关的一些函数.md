# html 编码解码
htmlentities()

简单来讲，HTML 标签用到的字符不能在 HTML作为普通字符直接使用，因为会影响到 HTML 的解析，如果要使用这些字符，就需要使用这些字符对应的HMTL实体（其实就是其他转义字符的概念。） 


然后在过滤用户输入时，可以根据情况使用 htmlentities 处理用户输入，防止恶意代码注入。



html_entity_decode
https://www.php.net/manual/zh/function.html-entity-decode.php


[HTML 字符实体][https://www.w3school.com.cn/html/html_entities.asp]
在 HTML 中，某些字符是预留的。
在 HTML 中不能使用小于号（<）和大于号（>），这是因为浏览器会误认为它们是标签。
如果希望正确地显示预留字符，我们必须在 HTML 源代码中使用字符实体（character entities）。


# url 编码解码
urldecode — 解码已编码的 URL 字符串
urlencode — 编码 URL 字符串


# http_build_query
http_build_query — 生成 URL-encode 之后的请求字符串


# parse_url
解析 URL，返回其组成部分


URL 函数
https://www.php.net/manual/zh/ref.url.php


# curl_getinfo
这个函数也在好好看看


# addcslashes
addcslashes() 函数返回在指定字符前添加反斜杠的字符串。

# htmlspecialchars
htmlspecialchars() 函数把预定义的字符转换为 HTML 实体。