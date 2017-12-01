# 简介
vue.js 的意义这里不在谈了，简而言之，就两点，数据与DOM的双向绑定和组件化，这两点有多牛，懂得人都懂。

这几主要对 vue.js 提供的特性做一个总结。


指令

指令带有前缀 v-，以表示它们是 Vue 提供的特殊属性

绑定 DOM 文本到数据，也可以绑定 DOM 结构到数据

而且，Vue 也提供一个强大的过渡效果系统，可以在 Vue 插入/更新/删除元素时自动应用过渡效果。


# 数据与DOM的双向绑定
1. 绑定 DOM 文本
2. 绑定 DOM 属性 v-bind
3. 绑定表单输入 v-model
4. 绑定事件监听器 v-on
5. 绑定 DOM 结构

## 绑定 DOM 文本

    <div id="app">
      {{ message }}
    </div>

    var app = new Vue({
      el: '#app',
      data: {
        message: 'Hello Vue!'
      }
    })

做了一个小实验，将上面 app.message 值改成了 "<h1>app</h1>"，发现 Vue 并没有进行渲染 h1 标签，仍然是把其当做字符串。
## 绑定 DOM 属性 v-bind

    <div id="app-2">
      <span v-bind:title="message">
        鼠标悬停几秒钟查看此处动态绑定的提示信息！
      </span>
    </div>

    var app2 = new Vue({
      el: '#app-2',
      data: {
        message: '页面加载于 ' + new Date().toLocaleString()
      }
    })

## 绑定表单输入 v-model

    <div id="app-6">
      <p>{{ message }}</p>
      <input v-model="message">
    </div>

    var app6 = new Vue({
      el: '#app-6',
      data: {
        message: 'Hello Vue!'
      }
    })

做了一个小实验，将上面的 

    <input v-model="message">

替换为了 

    <input type="text" :value="message">

并不行。
## 绑定事件监听器 v-on

    <div id="app-5">
      <p>{{ message }}</p>
      <button v-on:click="reverseMessage">逆转消息</button>
    </div>

    var app5 = new Vue({
      el: '#app-5',
      data: {
        message: 'Hello Vue.js!'
      },
      methods: {
        reverseMessage: function () {
          this.message = this.message.split('').reverse().join('')
        }
      }
    })

做了一个小实验，将上面

    <button v-on:click="reverseMessage">逆转消息</button>

替换为了

    <button :onclick="reverseMessage">逆转消息</button>

并不行。
## 绑定 DOM 结构
v-if; v-for

### v-if
    <div id="app-3">
      <p v-if="seen">现在你看到我了</p>
    </div>

    var app3 = new Vue({
      el: '#app-3',
      data: {
        seen: true
      }
    })
### v-for

    <div id="app-4">
      <ol>
        <li v-for="todo in todos">
          {{ todo.text }}
        </li>
      </ol>
    </div>

    var app4 = new Vue({
      el: '#app-4',
      data: {
        todos: [
          { text: '学习 JavaScript' },
          { text: '学习 Vue' },
          { text: '整个牛项目' }
        ]
      }
    })

# 组件
在 Vue 里，一个组件本质上是一个拥有预定义选项的一个 Vue 实例。

将数据从父作用域传到子组件


# 其他
数据与DOM的双向绑定与组件是 Vue.js 最核心的功能，但 Vue.js 也在这两者的基础上添加其他的一些相关功能。

1.  过渡效果系统

## 过渡效果系统
在 Vue 插入/更新/删除元素时自动应用过渡效果


# 细节
## Vue 实例
一个 Vue 应用由一个通过 new Vue 创建的根 Vue 实例，以及可选的嵌套的、可复用的组件树组成。

介绍了一个 Vue 实例包含了哪些选项、属性和方法，及这些选项、属性和方法的角色。

[Vue.js Api][3] 对这些东西做了很好的分类，可以随着学习的深入作为参考。
### 选项
Vue.js 将选项分为了如下几种:

1. 数据
    如 data、methods、watch、props、computed
2. DOM
    如 el、template
3. 生命周期钩子
    如 mounted、updated 等。比较重要的生命周期钩子 created、mounted、updated、destroyedS，使用时，需要我们对 Vue 实例的声明周期有一个认识。目前没必要是深入。
4. 资源
5. 组合
6. 其它
### 属性
目前明白如下属性是什么就可以了：

$data、$el

    var data = { a: 1 }
    var vm = new Vue({
      el: '#example',
      data: data
    })
    vm.$data === data // => true
    vm.$el === document.getElementById('example') // => true
    // $watch 是一个实例方法
    vm.$watch('a', function (newValue, oldValue) {
      // 这个回调将在 `vm.a` 改变后调用
    })

### 方法
Vue.js 将方法分为了如下几种:

1. 数据
    如 vm.$watch、vm.$set、vm.$delete
2. 事件
    如 vm.$on、vm.$once
3. 生命周期
    如 vm.$mount、vm.$nextTick

## 模板语法
在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，在应用状态改变时，Vue 能够智能地计算出重新渲染组件的最小代价并应用到 DOM 操作上。

如果你熟悉虚拟 DOM 并且偏爱 JavaScript 的原始力量，你也可以不用模板，直接写渲染 (render) 函数，使用可选的 JSX 语法。

NOTE(H)：目前对涉及到的这些底层东西先不深究。

布尔类特性、falsy

### 使用 JavaScript 表达式
对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。

    {{ number + 1 }}
    {{ ok ? 'YES' : 'NO' }}
    {{ message.split('').reverse().join('') }}
    <div v-bind:id="'list-' + id"></div>

    有个限制就是，每个绑定都只能包含单个表达式，所以下面的例子都不会生效。

    <!-- 这是语句，不是表达式 -->
    {{ var a = 1 }}
    <!-- 流控制也不会生效，请使用三元表达式 -->
    {{ if (ok) { return message } }}

NOTE:模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 Math 和 Date 。你不应该在模板表达式中试图访问用户定义的全局变量。


### 修饰符
修饰符 (Modifiers) 是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()：

    <form v-on:submit.prevent="onSubmit">...</form>

[1]: http://www.cnblogs.com/keepfool/p/5619070.html "vue.js——60分钟快速入门"
[2]: https://cn.vuejs.org/ "Vue.js"
[3]: https://cn.vuejs.org/v2/api/ "Vue.js Api"
