# 组件
## 如何正确的使用组件
组件必须在父实例的模块中以自定义元素的形式使用

比如下面这样就是一种错误的做法

      <my-component></my-component>
    <script type="text/javascript">
        // 注册
        Vue.component('my-component', {
            template: '<div>A custom component!</div>'
        });
    </script>

下面正确的做法：

    <div id="example">
        <my-component></my-component>
    </div>
    <script type="text/javascript">
    // 注册
    Vue.component('my-component', {
        template: '<div>A custom component!</div>'
    });

    // 创建根实例
    new Vue({
        el: '#example'
    });
    </script>

id 为 example 的 div 就是父模块，如果想要正确使用组件，就需要像上面这样，把组件放到父模块中

    <div id="example">
        <my-component></my-component>
    </div>

然后，为这父模块创建一个 vue 实例：

    new Vue({
        el: '#example'
    });

正确的使用组件，这两个条件缺一不可。

# DOM 模版解析说明
当使用 DOM 作为模版时 (例如，将 el 选项挂载到一个已存在的元素上), 你会受到 HTML 的一些限制，因为 Vue 只有在浏览器解析和标准化 HTML 后才能获取模版内容。尤其像这些元素 <ul>，<ol>，<table>，<select> 限制了能被它包裹的元素，而一些像 <option> 这样的元素只能出现在某些其它元素内部。

上面这段话，目前并不是很理解。但做了一个实验。

    <table id="example">
        <my-component></my-component>
    </table>
    <script type="text/javascript">
    // 注册
    Vue.component('my-component', {
        template: '<tr><td>A custom component!</td></tr >'
    });

    // 创建根实例
    new Vue({
        el: '#example'
    });
    </script>

上面这段代码，渲染后时这样的：

    <table id="example"></table>

我们使用的 my-component 没有任何效果，但代码改成这样就可以了


    <table id="example">
        <tr is="my-component"></tr>
    </table>
    <script type="text/javascript">
        // 注册
        Vue.component('my-component', {
            template: '<tr><td>A custom component!</td></tr >'
        });

        // 创建根实例
        new Vue({
            el: '#example'
        });
    </script>

这个问题先这样，以后在深入研究下。


