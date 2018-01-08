# 事件捕获
DOM0级事件处理程序

DOM2级事件处理程序

在DOM0级事件中，事件处理只有事件冒泡的过程，在IE8及其早期版本中只支持DOM0级事件。而W2C规范中的DOM2级事件规范中，规定了事件处理分为三个阶段：

1. 事件捕获阶段
2. 事件目标阶段
3. 事件冒泡阶段

NOTE: 真是 TM 的乱。

然后 jquery 为了解决浏览器的兼容问题，它又在原生的事件规范（DOM2级事件规范）中，加了很多自己的东西，具体特性如下：

1. 重定义了JQuery.Event对象, 统一了事件属性和方法, 统一了事件模型
2. 可以在一个事件类型上添加多个事件处理函数, 可以一次添加多个事件类型的事件处理函数
3. 支持自定义事件(事件命名空间)
4. 提供了toggle, hover组合事件
5. 提供了one, live-die, delegate-undelegate
6. 提供了统一的事件封装, 绑定, 执行, 销毁机制
7. $(document).ready();

真是日了狗。这块先研究到这里。


# return false
下面这个分别使用3中不同的方式给 id 为 baidu 的 a 标签添加 click 时间处理函数，然后在处理函数中返回 fasle。
三种方式返回 false 的效果分别如下：

1. 方式一：相当于即调用了 event.preventDefault() 又调用了 event.stopPropagation()。
2. 方式二：相当于调用了 event.preventDefault()
3. 方式三：既没有调用 event.preventDefault()，又没有调用 event.stopPropagation()。

只能说真是混乱啊。

    <!DOCTYPE html>
    <html>
    <head>
    <title>preventDefault example</title>
    </head>
    <body id="body">  
        <div id="box1" class="box1" style="height: 200px;background-color: gray;width: 400px">  
            <div id="box2" class="box2" style="height: 100px;background-color: red;width: 200px">  
                <span id="span">This is a span.</span>  
                <a href="https://www.baidu.com" id='baidu'>baidu</a>
            </div>  
        </div>  
    </body>

    <script type="text/javascript" src='assets/jquery/dist/jquery.js'></script>
    <script type="text/javascript">  
        window.onload = function() {  
        document.getElementById("box1").addEventListener("click",function(event){  
                alert("您好，我是最外层div。");  
                return false;
            });  
            document.getElementById("box2").addEventListener("click",function(event){  
                alert("您好，我是第二层div。");  
                return false;
            });  
            document.getElementById("span").addEventListener("click",function(event){  
                alert("您好，我是span。");  
                return false;
            });  

            // 方式一：jquery
            // $('#baidu').click((event) => {
            //   alert("您好，我是baidu。");
            //   return false;
            // })

            // 方式二：JavaScript
            // var a = document.getElementById("baidu");
            // a.onclick = function(){
            //   alert("您好，我是baidu。");
            //   return false;
            // };

            // 方式三：
            document.getElementById("baidu").addEventListener("click",function(event){  
                alert("您好，我是baidu。");

                return false;
            });  


        }  
    </script>  
    </html>


# event.stopPropagation
阻止事件继续冒泡。

# event.preventDefault
到底什么是默认操作？[《js事件(Event)之（四）阻止默认操作》][3]这篇 blog 解释的还可以。

默认操作与事件处理程序不是同一个概念，它是单击标签元素的默认操作。
如果不希望执行默认操作，那么在事件对象上调用 .stopPropagation() 方法也无济于事，因为默认操作不是在正常的事件传播流中发生的。

[1]: http://blog.csdn.net/luanlouis/article/details/23927347 "解析Javascript事件冒泡机制"
[2]: https://developer.mozilla.org/zh-CN/docs/Web/API/Event/preventDefault "event.preventDefault"
[3]: https://www.cnblogs.com/yycode/articles/5499234.html "js事件(Event)之（四）阻止默认操作"
[5]: https://itbilu.com/javascript/js/Vy1jsXqJg.html "JavaScript DOM文档事件－DOM0、DOM2级事件处理程序"
[4]: https://itbilu.com/javascript/js/Ek6pnznye.html "JavaScript DOM事件处理－事件捕获和事件冒泡"
