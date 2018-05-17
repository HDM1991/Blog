# 什么是字符实体
character entities

字符实体类似这样：

&entity_name;

或者

&#entity_number;


比如小于号对应的字符实体是 &lt; 或 &#60;

# 两者的区别
> This function is identical to htmlspecialchars() in all ways, except with htmlentities(), all characters which have HTML character entity equivalents are translated into these entities.

htmlentities 会将所有拥有 HTML 实体的字符转换对应的 HTML 实体；而 htmlspecialchars 只会将如下几个字符转换为对应的 HTML 实体。

* &
* "
* '
* <
* >

那都有哪些字符有 HTML 实体呢？[《HTML ISO-8859-1 参考手册
》][1]提供了所有拥有 HTML 实体的字符列表。

# encoding 参数
表示要进行转换的字符串的字符集。通常不用设置。除非跟 php.ini 中 default_charset 选项的值不一样。

# flags 参数
ENT_IGNORE、ENT_SUBSTITUTE 这两个选项注意下，两者都用于处理字符串中无法识别的字符，前者将其从结果字符串中丢弃，后者将其替换成 Unicode Replacement Character U+FFFD (UTF-8) or &#xFFFD。

如果进行转换的字符串中存在无法识别的字符，并且没有指定如上两个参数，函数会返回一个空字符串。

[1]: http://www.w3school.com.cn/tags/html_ref_entities.html "《HTML ISO-8859-1 参考手册"