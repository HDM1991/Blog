# white-space
The [white-space][1] CSS property determines how whitespace inside an element is handled.

whitespace 具体指什么？空格、制表符、换行符。

总结来讲，whitespace 决定了浏览器如何处理元素中的 whitespace (即空格、制表符、换行符)，还有就是在文本长度超出元素的宽度时，是否自动换行显示。

下面这张表总结的很好。New lines 指换行符；Text wrapping 指文本长度超出元素的宽度时如何处理。

            New lines   Spaces and tabs     Text wrapping
normal      Collapse    Collapse            Wrap
nowrap      Collapse    Collapse            No wrap
pre         Preserve    Preserve            No wrap
pre-wrap    Preserve    Preserve            Wrap
pre-line    Preserve    Collapse            Wrap


[1]: https://developer.mozilla.org/en-US/docs/Web/CSS/white-space "white-space"