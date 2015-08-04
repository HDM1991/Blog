
# 前言
这篇文章主要是解答内心的一些疑问。因为遇到和这些东西相关的情况不是一次两次了，彻底解决下。正式开始之前，简单的说下我遇到的一些情况，大家也可以思考下，如果自己面临这些情况如何解决？

1. 在公司时曾经做过一个安卓项目，需要在基于 C++，采用 VS 开发的服务端和基于 Java 开发的客户端之间传递数据。当然，这些数据中包括字符串，怎么做？

    我提供解决方案是这样的，服务端字符串采用宽字符串，也就是做 *wchar_t* 类型，因为在看了Java String 类的文档后，我发现 Java String 类可以根据字节数组和指定的字符集编码初始化一个 String 对象，而且 String 类有一个成员函数可以基于指定的字符集编码返回一个字节数组。虽然当时对 *wchar_t* 类型的理解并不深，但直觉上还是觉得 *wchar_t* 类型使用的字符集编码应该是某种 Unicode 字符集编码，只要确定了这个字符集编码，我就解决了这个问题。于是我就一个一个试了下，最后确定为 UTF-16BE。
    
2. 当时公司一个同事在做一个 Shell 插件，结果好像当这个 Shell 插件的服务端运行在使用韩文的电脑上，客户端运行在使用中文的电脑上时，客户端就会出现乱码的现象？

    这个问题的大致情况就是这样。因为没参与这个项目，所以具体情况也不太清楚。但现在再看这个问题，我们可以思考下为什么会出现乱码，我觉得原因应该是如下情况之一：

    1. Shell 插件的服务端和客户端使用的 OEM 代码页（也即是字符集编码）不一致
    2. 虽然 Shell 插件的服务端和客户端使用的 OEM 代码页一致，比如都是 949（韩文 OEM 代码页的编号），但运行 Shell 插件客户端的电脑并没有这种代码页的支持。
    
    导致问题的原因清楚了，如何解决？我的思路是这样的，首先还是要保证 Shell 插件的客户端和服务端使用的 OEM 代码页一致，这是前提。但关于使用什么 OEM 代码页我有不同的想法，我会首先尝试某种 Unicode 代码页，比如 UTF8，为什么？因为我觉得这样应该可以避免在运行 Shell 插件客户端的代码上在安装某种代码页支持，这是我的直觉。
3. 程序中需要将 char 类型的字符串转会为 wchar_t 类型的字符串，或者相反。


# wchar_t 类型的长度和使用的字符集编码
*wchar_t* 类型的长度一定是 2 个字节吗？*wchar_t* 类型使用的字符集编码一定是固定的吗？都不是，事实是 *wchar_t* 类型的长度和所使用的字符编码都是由编译器决定的。比如在 VS 2013 中，*wchar_t* 类型的长度为 2 个字节，所使用的字符集编码为 UTF-16BE；而在 GCC 下，*wchar_t* 的长度为 4 个字节，所使用的字符集编码为 UTF-32BE。为什么会这样呢？原因主要就是因为标准中没有明确定义 wchar_t 类型的长度和使用的字符集编码。具体情况大家可以参考如下文章：

* [刨根问底：C++中宽字符类型（wchar_t）的编码一定是Unicode？长度一定是16位？
](http://www.ituring.com.cn/article/111027)
* [维基百科：宽字符](https://zh.wikipedia.org/wiki/%E5%AF%AC%E5%AD%97%E5%85%83)

# char 类型使用的字符集编码
这个问题更确切的问法是，在 Windows 平台下，应用程序运行时使用什么字符集编码来解释程序中的 char 类型字符串？答案是取决于系统设置。具体就是指 Windows 控制面板中的区域设置，如下所示:

![Windows 区域设置](http://img.blog.csdn.net/20150701160714462 "Windows 区域设置")

更详细的情况大家可以参考如下文章:

* [谈谈Windows程序中的字符编码](https://raw.githubusercontent.com/HDM1991/Blog/master/%E5%AD%97%E7%AC%A6%E9%9B%86%E5%92%8C%E5%AD%97%E7%AC%A6%E9%9B%86%E7%BC%96%E7%A0%81%E6%9D%82%E8%B0%88%E7%B3%BB%E5%88%97%E4%B9%8Bwchar_t%EF%BC%8Cchar%EF%BC%8C%E4%BB%A3%E7%A0%81%E9%A1%B5/local.png)

# 代码页
说到 Windows 下的字符集编码，就不能不说代码页这个概念。代码页其实就是字符集编码，一个代码页代表一种字符集编码，只不过每一个代码页都由一个唯一的编号标识，或者说在代码页的概念中，每一种字符集编码都由一个唯一的编号标识。那谁来规定哪个数字来代表哪个字符集编码呢？这是一个问题，一个标准制定的问题。很不幸，在 Windows 平台下存在两种标准，一种是 OEM 代码页；一种是 Windows 代码页（Windows ANSI code page）。

OEM 代码页是由 IBM 制定的一系列代码页合集。OEM 代码页在 Windows 平台下主要供控制台程序使用。Windows 代码页是由 Windows 制定的一系列代码页合集，GUI 应用程序运行时使用的代码页就是 Windows 代码页。至于为什么会出现这两种代码页，这里就不细说了。我觉得，就像上面说的，这是一个标准制定的的问题，应该说没有公司会在标准制定方面让步，大家都想把标准制定的权利掌握在自己手中，当然前提是得有实力。 具体情况大家可以参考如下文章：

* [维基百科：代码页](https://zh.wikipedia.org/wiki/代码页)。
* [Windows代码页](http://blog.wuliaoa.com/?p=495)

# Windows ANSI code page
OK ，现在让我们说说上面提到的 Windows 代码页，在 Microsoft 的文档中，它的全称叫 Windows ANSI code page。为什么要单独说说 Windows ANSI code page 呢？因为它真的很容易让人误解。第一次看到它时，因为 ANSI 这个词的存在，我觉得它应该指的是某个代码页，然而并不是。但我还是搞不懂为什么由 Windows 制定的一系列代码页要叫 Windows ANSI code page，这跟 ANSI 有什么关系？事实是 Windows ANSI code page 确实不是一个正确的叫法。下面是微软官方文档中的解释。

> ANSI: Acronym for the American National Standards Institute. The term “ANSI” as used to signify Windows code pages is a historical reference, but is nowadays a misnomer that continues to persist in the Windows community. The source of this comes from the fact that the Windows code page 1252 was originally based on an ANSI draft—which became International Organization for Standardization (ISO) Standard 8859-1. “ANSI applications” are usually a reference to non-Unicode or code page–based applications.

# MultiByteToWideChar 和 WideCharToMultiByte
关于这两个函数主要说下他们的第一个参数 CodePage 可以传递的 *CP_ACP* 这个值。下面是这两个函数的文档中对这个值的描述。

> The system default Windows ANSI code page.

OK，现在我们就解释下这句话。首先 Windows ANSI code page，上面已经提到它指由 Windows 制定的一系列代码页了。那什么是系统默认的 Windows 代码页呢？其实这个问题上面也已经提到了，它是由 Windows 控制面板中的区域设置决定的。另外，就这两个函数的一般用途来讲，基本上我们很少用到除 *CP_ACP* 之外的值。

# 思考
假如有这样一个开发平台，你会不会觉得很方便呢？首先，该开发平台下只有字符和字符串的概念，没有什么 char 类型，wchar_t 类型，多字节字符串和宽字节字符串；然后该平台下编译的程序原生支持多语言，你再也不用担心程序的国际化问题了。

实现这样一个开发平台难吗？我不知道，但我觉得只要做好如下两点就可以了：

1. 该平台提供的所有 API 必须跟什么 char，wchar_t 类型，多字节字符串和宽字节字符串无关
2. 该平台原生支持 Unicode，内部实现采用 Unicode。

其实这里的主要问题就是第一点。为什么我们在 C 和 C++ 程序的开发中会遇到关于字符集编码的问题，主要原因就是因为我们使用的 API 因为历史原因是跟 char 类型，wchar_类型，多字节字符串和宽字节字符串相关的。就 Windows 来讲，它内部其实已经采用了 Unicode 编码，但因为在这些 API 的原因，我们还需要处理一些关于字符集编码的问题。

另外，虽然对 Java 不太了解，但貌似在这类新型平台下，好像已经不需要关心字符串的类型了。哎，谁让人家比较年轻呢？

最后说一句，Unicode 是好东西，希望有一天 char，wchar_t ，代码页能消失。