有两种基本的 XML 解析器类型：

基于树的解析器：这种解析器把 XML 文档转换为树型结构。它分析整篇文档，并提供了 API 来访问树种的元素，例如文档对象模型 (DOM)。
基于事件的解析器：将 XML 文档视为一系列的事件。当某个具体的事件发生时，解析器会调用函数来处理。


基于事件的解析器集中在 XML 文档的内容，而不是它们的结果。正因如此，基于事件的解析器能够比基于树的解析器更快地访问数据。

# PHP XML Expat 解析器
[XML Expat][1]是基于事件的解析器。另外它是 PHP 核心的组成部分。无需安装。
Expat 不检查有效性，忽略任何 DTD。

[PHP XML Expat][1]提供的例子在一定程度上很好的说明了 XML Expat 的是否用方式，使用情景。

# PHP XML DOM
同样无需安装。

W3C DOM 提供了针对 HTML 和 XML 文档的标准对象集，以及用于访问和操作这些文档的标准接口。

W3C DOM 被分为不同的部分 (Core, XML 和 HTML) 和不同的级别 (DOM Level 1/2/3)：

* Core DOM - 为任何结构化文档定义标准的对象集
* XML DOM - 为 XML 文档定义标准的对象集
* HTML DOM - 为 HTML 文档定义标准的对象集

当 XML 生成时，它通常会在节点之间包含空白。XML DOM 解析器把它们当作普通的元素。

# PHP SimpleXML
不好用，就不说了。


# 总结
上述提到的 3 种 XML 解析器，都无法满足我们的需求，即像 jquery 那样操作 dom。所以还是用 [DiDOM][2]。

[1]: http://www.w3school.com.cn/php/php_xml_parser_expat.asp "XML Expat"
[2]: https://github.com/Imangazaliev/DiDOM