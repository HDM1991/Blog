# 对象的引用、复制
ECMAScript的变量值类型共有两种：

基本类型 (primitive values) - 包括Undefined, Null, Boolean, Number和String五种基本数据类型
引用类型 (reference values) - 保存在内存中的对象们，不能直接操作，只能通过保存在变量中的地址引用对其进行操作

## clone_.clone(object) 
underscore.js 提供了 clone 函数用于实现对象的浅拷贝。

Create a shallow-copied clone of the provided plain object. Any nested objects or arrays will be copied by reference, not duplicated.

    _.clone({name: 'moe'});
    => {name: 'moe'};


[1]: http://hellobug.github.io/blog/javascript-variable-assignment/ "[JS] 让人犯晕的JavaScript变量赋值"
[2]: https://my.oschina.net/u/171860/blog/723918 "  $.extend复制特点"