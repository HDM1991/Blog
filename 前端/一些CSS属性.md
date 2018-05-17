# display
## inline
width、height 属性无效

## inline-block
[inline-block][3] elements are like inline elements but they can have a width and a height.


# word-break
break-all

[word-break][http://www.w3school.com.cn/cssref/pr_word-break.asp]

# line-height
line-height 属性设置行间的距离（行高）。

该属性会影响行框的布局。

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


# writing-mode




# word-wrap

# cursor
cursor 属性规定要显示的光标的类型（形状）。

该属性定义了鼠标指针放在一个元素边界范围内时所用的光标形状

[1]: https://developer.mozilla.org/en-US/docs/Web/CSS/white-space "white-space"
[2]: http://www.w3school.com.cn/cssref/pr_dim_line-height.asp "CSS line-height 属性"
[3]: https://www.w3schools.com/css/css_inline-block.asp "CSS Layout - inline-block"