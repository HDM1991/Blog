与传统的函数语法除了语法方面还有如下几点区别：

1. 与父作用域共享关键字 this
2. 没有 arguments 变量


箭头函数表达式的语法比函数表达式更短，并且不绑定自己的this，arguments，super或 new.target。


其实想想，对我们来讲，最有用的点就是与父作用域共享关键字 this 和 简洁的语法。其他区别点，暂时不用关心。



[1]: https://segmentfault.com/a/1190000003781467 "JavaScript ES6箭头函数指南"
[2]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arrow_functions "箭头函数"