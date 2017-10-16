# 前言
不得不说，art-template 很强大，但就我们目前的情况而言（目前只考虑在浏览器中直接使用的情况），用到的东西并不多。如下：

1. template(filename, content)
    注意一点，浏览器版本无法加载外部文件，filename 为存放模板的元素 id。
2. 过滤器
3. 模板继承
4. 子模板
5. 调试
6. 模板变量

# 疑惑点
目前比较疑惑主要有下面这些点：

1. 模板中能否直接使用自定义的全局变量
    经过测试不能

        <h1>{{ g_info }}</h1>
2. 模板中能否直接使用全局函数
    经过测试不能

    <h1>{{ lower(data) }}</h1>

3. 模板中能否直接使用一些特殊的操作符，比如 instanceof、typedef
    经过测试，instanceof 不行；typedef 可以

        <h1>{{ data instanceof Array ? 'yes' : 'no' }}</h1>

        <h1>{{ typeof data_array }}</h1>
        <h1>{{ typeof data }}</h1>
4. 模板中能否使用对象的方法
    经过测试，可以。

        <h1>{{ data.toLowerCase() }}</h1>
        <h1>{{ data_array.join(';') }}</h1>

5. art-template 标准语法与原始语法的区别
    标准语法支持基本模板语法以及基本 JavaScript 表达式；原始语法支持任意 JavaScript 语句。但目前我们对这句话并没有什么深入的了解。

出现上述困惑的原因，主要还是对 art-template 引擎的原理没什么了解。但考虑到目前的需求情况，暂时就不深入研究了，目前掌握的东西已经够用了。

然后，上述说说的这些情况中的 1、2、3 都是可以通过 template.defaults.imports 将函数或者变量进行导入来解决的。比如下面这个例子：
    
    <script id="demo" type="text/html">
      <h1>{{ g_info }}</h1>
      <h1>{{ Math.abs(7.25) }}</h1>
      <h1>{{ lower(data) }}</h1>
    </script>

    <script type="text/javascript">

      var g_info = 'hello';

      function lower(v) {
        return v.toLowerCase();
      }

      template.defaults.imports.lower = lower;
      template.defaults.imports.Math = Math;
      template.defaults.imports.g_info = g_info;

      var html = template('demo', {
          data: 'DEMO',
          data_array: new Array("Saab","Volvo","BMW")
      });

      $('#content').append(html);
    </script>

我们分别导入了一个名为 lower 的全局函数、一个名为 g_info 的全局变量、JavaScript 的 Math 对象，然后我们在模板中就可以直接使用他们。


# 例子
    <div id="content"></div>
    <script id="demo" type="text/html">
      <h1>{{ g_info }}</h1>
      <h1>{{ typeof data_array }}</h1>
      <h1>{{ Math.abs(7.25) }}</h1>
      <h1>{{ data.toLowerCase() }}</h1>
      <h1>{{ data_array.join(';') }}</h1>
      <h1>{{ data_array[0] }}</h1>
      <h1>{{ $data['data'] }}</h1>
    </script>

    <script type="text/javascript">

      var g_info = 'hello';

      function lower(v) {
        return v.toLowerCase();
      }

      template.defaults.imports.lower = lower;
      template.defaults.imports.Math = Math;
      template.defaults.imports.g_info = g_info;

      var html = template('demo', {
          data: 'DEMO',
          data_array: new Array("Saab","Volvo","BMW")
      });

      $('#content').append(html);
    </script>

