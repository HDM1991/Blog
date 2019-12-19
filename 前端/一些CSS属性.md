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

该属性会影响行框的布局。应用到一个块级元素时，它定义了该元素中基线之间的最小距离而不是最大距离。
line-height 与 font-size 的计算值之差（在 CSS 中成为“行间距”）分为两半，分别加到一个文本行内容的顶部和底部。可以包含这些内容的最小框就是行框。

# white-space
The [white-space][1] CSS property determines how whitespace inside an element is handled.

whitespace 具体指什么？空格、制表符、换行符。

总结来讲，whitespace 决定了浏览器如何处理元素中的 whitespace (即空格、制表符、换行符)，还有就是在文本长度超出元素的宽度时，是否自动换行显示。

下面这张表总结的很好。New lines 指换行符；Text wrapping 指文本长度超出元素的宽度时如何处理。

          换行符      空格和制表符    当行长度超过元素长度时，是否wrap
normal    合并        合并          是
nowrap    合并        合并          否
pre       保留        保留          否
pre-wrap  保留        保留      是
pre-line  保留        合并      是

什么叫合并，对于空格和制表符，不管是连续多少个空格和制表符，浏览器显示时只显示一个空格；对于换行符，浏览器不会换行，也是显示一个空格。

什么叫保留。对于空格和制表符，就是有多少个就显示多少了；对于换行符，就是正常换行。





# writing-mode




# word-wrap

# cursor
cursor 属性规定要显示的光标的类型（形状）。

该属性定义了鼠标指针放在一个元素边界范围内时所用的光标形状

# background-image
background-size
https://www.w3school.com.cn/cssref/pr_background-size.asp

background-repeat
https://www.w3school.com.cn/cssref/pr_background-repeat.asp

background-position
https://www.w3school.com.cn/cssref/pr_background-position.asp

[1]: https://developer.mozilla.org/en-US/docs/Web/CSS/white-space "white-space"
[2]: http://www.w3school.com.cn/cssref/pr_dim_line-height.asp "CSS line-height 属性"
[3]: https://www.w3schools.com/css/css_inline-block.asp "CSS Layout - inline-block"