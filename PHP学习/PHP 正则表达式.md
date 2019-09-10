PHP 中正则表达式有三种用法：

* 匹配，从某个字符抽取信息
* 用新文本替换匹配的文本
* 把字符串拆分成小块字符串组成的数组

然后 PHP 使用的 C 语言库是 PCRE，这个库几乎可以全部支持 Perl 正则表达式的所有特性。


# 子模式
这个东西可以这样理解，首先先匹配整个正则表达式，之后在完成匹配的字符串的基础上，在匹配正则表达式中的子模式。

这样我们不但可以获取匹配的字符串，还可以根据需要通过子模式获取匹配字符串中想要的部分。

# 分隔符
Perl 样式的正则表达式的语法要求每种模式都必须包含在一对分隔符中。传统上，使用 / 作为分隔符。但其实任何非字母数字字符都可以当做分隔符。

如果我们的正则表达式中要包含 /，比如匹配文件名的正则表达式，就可以使用这种特性。比如下面示例中两个正则表达式其实是一样的。

    preg_match("/\/usr\/local\//", '/usr/local/bin/perl');
    preg_match("#/usr/local/#", '/usr/local/bin/perl');


# 字符类
就是类似 [0-9a-zA-Z] 这种通用的起的一个简单、明了的别名，比如 [0-9a-zA-Z] 对应的别名是 [:alnum:]

然后这些别名还可以组合起来用

比如 [Z[:digit:][:lower:]] 匹配字母 Z、数字、小写字母，等于家[Z0-9a-z]

然后还有下面这种，可以匹配字母 s、t 或者字符串 ch，[.ch.] 这种写法成为排序序列(collating sequences)

    [st[.ch.]]

# 锚
锚(anchor)将匹配限制在字符串中的特定位置，比如 ^、$ 都是锚。


# 量词和贪婪
贪婪模式

    preg_match("/(<.*>)/", "do <b>not</b> press the button", $match);
    # $match[1] 的值是 '<b>not</b>'

非贪婪模式

    preg_match("/(<.*?>)/", "do <b>not</b> press the button", $match);
    # $match[1] 的值是 '<b>'

两者之间的区别通过上面的例子可以很清楚的看出来。

## 非捕获组
就是有些情况下我们需要用()将正则表达式的一部分括起来，但是又不想这部分被当做子模式，那就可以用?:

# 逆向引用

    $r = preg_match('/([[:alpha:]]+)\s+\1/', "Paris in the the spring", $matches);

上面这个例子，$matches 的值如下：

    array(2) {
      [0] =>
      string(7) "the the"
      [1] =>
      string(3) "the"
    }

# 后缀选项和内联选项
就是在正则表达式中添加一个选项影响正则表达是的匹配规则，比如是否忽略字母大小写，是否忽略（正则表达式中的）空白和注释等。

# 前向和后向断言
NOTE: 好好看下，在将字符串分割成数据的情景中很有用

# 修饰符
m (PCRE_MULTILINE)

默认情况下，PCRE 认为目标字符串是由单行字符组成的(然而实际上它可能会包含多行)， "行首"元字符 (^) 仅匹配字符串的开始位置， 而"行末"元字符 ($) 仅匹配字符串末尾， 或者最后的换行符(除非设置了 D 修饰符)。这个行为和 perl 相同。 当这个修饰符设置之后，“行首”和“行末”就会匹配目标字符串中任意换行符之前或之后，另外， 还分别匹配目标字符串的最开始和最末尾位置。这等同于 perl 的 /m 修饰符。如果目标字符串 中没有 "\n" 字符，或者模式中没有出现 ^ 或 $，设置这个修饰符不产生任何影响。


u (PCRE_UTF8)
此修正符打开一个与 perl 不兼容的附加功能。 模式和目标字符串都被认为是 utf-8 的。 无效的目标字符串会导致 preg_* 函数什么都匹配不到； 无效的模式字符串会导致 E_WARNING 级别的错误。 PHP 5.3.4 后，5字节和6字节的 UTF-8 字符序列被考虑为无效（resp. PCRE 7.3 2007-08-28）。 以前就被认为是无效的 UTF-8。


This modifier turns on additional functionality of PCRE that is incompatible with Perl. Pattern and subject strings are treated as UTF-8. An invalid subject will cause the preg_* function to match nothing; an invalid pattern will trigger an error of level E_WARNING. Five and six octet UTF-8 sequences are regarded as invalid since PHP 5.3.4 (resp. PCRE 7.3 2007-08-28); formerly those have been regarded as valid UTF-8.



# 相关函数
preg_match

preg_match_all 从最后一个匹配末尾重复的匹配

## 替换
preg_replace 可以在替换字符串中使用逆向引用，然后语法是 $1、$2、$3 这样。

## 拆分


# 中文处理
https://www.php.net/manual/zh/intro.regex.php
从PHP5.3.0开始本扩展不再建议使用，所有调用本扩展中函数都将提示E_DEPRECATED 错误。

preg和ereg的区别
https://blog.csdn.net/tonyxf121/article/details/7738850

总结来京，preg和ereg 语法不一样，ereg 已经被官方抛弃，不建议在使用。



PHP 正则匹配汉字的正确姿势
http://notes.cribug.com/2016/RegEx-Han/