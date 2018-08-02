# Window 对象
[Window 对象][1]表示浏览器窗口。

所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员。

全局变量是 window 对象的属性。

全局函数是 window 对象的方法。

如果文档包含框架（frame 或 iframe 标签），浏览器会为 HTML 文档创建一个 window 对象，并为每个框架创建一个额外的 window 对象。

# 客户端窗口重定向
有如下几种方法
1. window.location.href="http://shanghepinpai.com"; 
2. window.location.replace("http://www.w3school.com.cn")
3. window.location.assign("http://www.w3school.com.cn")

建议使用方法2或者方法3，比较标准。另外方法2和方法3之间的区别就是 replace() 方法不会在 History 对象中生成一个新的记录。当使用该方法时，新的 URL 将覆盖 History 对象中的当前记录，直白的说，假如当前窗口打开的网址是 www.bing.com，用 window.location.assign("http://www.w3school.com.cn") 重定向到 http://www.w3school.com.cn，该窗口就会产生一条历史记录 www.bing.com；但使用 replace() 方法就不会。


# Navigator 对象
Navigator 对象包含有关浏览器的信息。

注意下 appVersion 属性，可以用来判断浏览器版本种类。示例如下：

    window.navigator.appVersion
    "5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36"

[1]: http://www.w3school.com.cn/jsref/dom_obj_window.asp "Window 对象"
[2]: http://www.w3school.com.cn/jsref/met_loc_replace.asp "HTML DOM replace() 方法"
[3]: http://www.w3school.com.cn/jsref/dom_obj_navigator.asp "Navigator 对象"

