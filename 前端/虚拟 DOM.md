如果对前端工作进行抽象的话，主要就是维护状态和更新视图；而更新视图和维护状态都需要DOM操作。其实近年来，前端的框架主要发展方向就是解放DOM操作的复杂性。

虚拟的DOM的核心思想是：对复杂的文档DOM结构，提供一种方便的工具，进行最小化地DOM操作。

既然我们可以用JS对象表示DOM结构，那么当数据状态发生变化而需要改变DOM结构时，我们先通过JS对象表示的虚拟DOM计算出实际DOM需要做的最小变动，然后再操作实际DOM，从而避免了粗放式的DOM操作带来的性能问题。

[1]: http://www.ituring.com.cn/article/211352 "全面理解虚拟DOM，实现虚拟DOM
"