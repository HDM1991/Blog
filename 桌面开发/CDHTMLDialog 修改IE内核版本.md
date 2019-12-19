CDHTMLDialog 默认的 IE 模式为 IE7，可以通过修改注册表的方式使用不同的 IE 模式。


具体方法：
对于 32 位应用程序，
在 32 位windows系统上，在 HKLM\SOFTWARE\Microsoft\Internet Explorer\Main\FeatureControl\FEATURE_BROWSER_EMULATION 键下新建一个名字为使用了 CDHTMLDialog 的程序的文件名，类型为 DWORD 的注册表值，然后根据自己的需要设置为该注册表值设置不同的值。

如果是 64 位windows系统，在 HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Internet Explorer\Main\FeatureControl\FEATURE_BROWSER_EMULATION 



注意，CDHTMLDialog 使用的IE内核即系统上IE，如果将 CDHTMLDialog 使用的 IE 模式设置为比系统的IE版本高，将不会起作用。


[1]: https://docs.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/general-info/ee330730(v=vs.85)#browser-emulation "Internet Feature Controls"