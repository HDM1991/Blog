https://segmentfault.com/a/1190000006178770

https://www.webpackjs.com/guides/


# 生成Source Maps（使调试更容易）
对小到中型的项目中，用 eval-source-map 就可以。

    module.exports = {
      devtool: 'eval-source-map',
      entry:  __dirname + "/app/main.js",
      output: {
        path: __dirname + "/public",
        filename: "bundle.js"
      }
    }

# 使用webpack构建本地服务器
就是一个简单的 http server，但是会监听代码改变自动刷新网页，不用在手动刷新了。


# Loaders
通过使用不同的loader，webpack有能力调用外部的脚本或工具，实现对不同格式的文件的处理，比如说分析转换scss为css，或者把下一代的JS文件（ES6，ES7)转换为现代浏览器兼容的JS文件，对React的开发而言，合适的Loaders可以把React的中用到的JSX文件转换为JS文件。

这个功能好。

Loaders需要单独安装并且需要在webpack.config.js中的modules关键字下进行配置，Loaders的配置包括以下几方面：

test：一个用以匹配loaders所处理文件的拓展名的正则表达式（必须）
loader：loader的名称（必须）
include/exclude:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
query：为loaders提供额外的设置选项（可选）


# Babel
Babel其实是一个编译JavaScript的平台，它可以编译代码帮你达到以下目的：

* 让你能使用最新的JavaScript代码（ES6，ES7...），而不用管新标准是否被当前使用的浏览器完全支持
* 让你能使用基于JavaScript进行了拓展的语言，比如React的JSX；

# 一切皆模块
Webpack 把所有的文件都都当做模块处理，JavaScript代码，CSS和fonts以及图片等等通过合适的loader都可以被处理。



# 常见问题
[在配置webpack.config.js自动打包的时候,出现Error: Cannot find module '@babel/core'错误][2]

[1]: "http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html" "JavaScript Source Map 详解"

[2]: "https://www.cnblogs.com/soyxiaobi/p/9554565.html" "
在配置webpack.config.js自动打包的时候,出现Error: Cannot find module '@babel/core'错误"