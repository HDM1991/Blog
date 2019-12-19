将 serialize 后的值直接以文本的形式存储到数据库时需要注意的点，如果要进行 serialize 的值中包含引号，直接存到数据库，那以后取出进行 unserialize 时会出错。
https://www.oschina.net/code/snippet_117002_11293


[1]: http://justcode.ikeepstudying.com/2015/10/php%E6%95%B0%E7%BB%84%E7%BC%93%E5%AD%98%E4%B8%89%E7%A7%8D%E6%96%B9%E5%BC%8Fjson%E3%80%81%E5%BA%8F%E5%88%97%E5%8C%96%E5%92%8Cvar_export%E7%9A%84%E6%AF%94%E8%BE%83/ "PHP数组缓存:三种方式JSON、序列化和var_export的比较"
