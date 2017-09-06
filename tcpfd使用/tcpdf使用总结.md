# 简介
这篇博客主要是记录 [tcpdf][1] 在使用中的一些要点和注意事项。这里使用的 tcpdf 版本是 6.2.13。

# Fonts
## 使用方式
There are two ways to use a new font: embedding it in the PDF (with or without subsetting) or not.

两种使用字体的方式：在 PDF 文件中嵌入字体和不嵌入字体。在 PDF 中嵌入字体的好处是，即使查看这个 PDF 的 PC 上没有安装这个 PDF 用到的字体，这个 PDF 也能正常显示，但缺点就是因为在 PDF 文件中嵌入了字体，PDF 文件的体积会增大；不嵌入字体这种方式则相反，两种方式可以根据实际情况选择使用。

然后嵌入字体这种方式，又分为在 PDF 文件中嵌入所用字体的一个子集和完全嵌入。嵌入子集能减小 PDF 文件中体积，但是限制了对这个 PDF 进行修改的能力，如果我们在修改时键入了一个子集中不包含的字符，这个字符就无法显示。

## 中文字体
如果我们要在 pdf 中打印中文，就需要设置中文字体，tcpdf 默认支持两种中文字体：stsongstdlight 和 cid0cs。但我们可以使用 tcpdf 基于 ttf 字体文件生成字体的功能，可以自由使用其他字体。

# 打印 HTML 注意事项
当我们使用 tcpdf 打印 html 时，我们会发现，同一段 html 代码通过 tcpdf 打印出来的效果，和通过浏览器查看的效果并不一样；当你进一步尝试为这段 html 代码添加一些 css 控制其表现时，你又会发现，添加的很多 css 并没有起任何作用。Why？原因和同一段 html 代码通过 chrome 查看和通过 ie 查看效果不一样相同，因为浏览器要渲染 html，chrome 和 ie 的渲染引擎不同，自然表现就不一样，而且对 css 的支持也不一样。同样，tcpdf 也是自己渲染 html，所以同一段 HTML 代码的渲染效果和浏览器不同也是正常的，然后对于很多 CSS 不支持也正常，因为毕竟 tcpdf 不是浏览器，又是自己解释 html，不可能做到像浏览器那样。

ok，现在我们知道，tcpdf 对 html 的渲染效果和浏览器有差别，支持的 CSS 有限，支持的 HTML 标签也有限。那怎样在使用 tcpdf 打印 pdf 时尽可能的调整出自己想要的效果呢？答案就是搞清楚一些坑，然后多尝试。下面就谈谈我遇到的一个坑。

## 标签的间距
首先，tcpdf 并不支持 CSS margin 属性；然后 tcpdf 为每个 html 标签都设置了默认的 margin；最后我们通过 setHtmlVSpace 函数对每个标签的 margin 进行调整。

这里面最大的问题在于 tcpdf 为每个 html 标签设置的默认 margin 并不合适，比如 tcpdf 竟然为 div 标签也设置了 margin，fuck；然后 tcpdf 对间距的计算是直接累加的，不会像浏览器那样根据实际情况对间距进行处理。

所以，怎么弄处理标签的间距很重要。没有固定方法，比如我的做法就是通过 setCellHeightRatio 函数控制行间距，在额外利用换行符对间距进行控制的。大家可以根据自己的情况指定相应的解决方案。

[1]: https://tcpdf.org/ "TCPDF"