# 常用 api
就是 ajax
[wx.request(Object object)][https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html]

设置导航栏的标题
[wx.setNavigationBarTitle(Object object)][https://developers.weixin.qq.com/miniprogram/dev/api/ui/navigation-bar/wx.setNavigationBarTitle.html]

# tabbar
[tabBar][https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#tabBar]

一般来讲，在 app.json 进行配置就行了，然后 tabbar 也可以进行定制，但这个就需要写代码了，但一般也用不到。

[自定义 tabBar][https://developers.weixin.qq.com/miniprogram/dev/framework/ability/custom-tabbar.html]


自定义说白了就如下几点。

首先还得在 app.json 配置 tabBar 字段，这样做的原因首先是告诉微信，当前 tabbar 是定制的；其次微信要通过 list 字段里面的信息知道哪些页面要有 tabbar，因为指定以 tabbar 本质上就是自定义了一个组件，它不知道哪些页面要有 tabbar

        
    "tabBar": {
      "custom": true,   // 告诉微信，当前 tabbar 是定制的
      "color": "#7A7E83",
      "selectedColor": "#3cc51f",
      "borderStyle": "black",
      "backgroundColor": "#ffffff",
      "list": [
        {
          "pagePath": "index/index",
          "iconPath": "image/icon_component.png",
          "selectedIconPath": "image/icon_component_HL.png",
          "text": "组件"
        },
        {
          "pagePath": "index/index2",
          "iconPath": "image/icon_API.png",
          "selectedIconPath": "image/icon_API_HL.png",
          "text": "接口"
        }
      ]
    },

其次，对于自定义 tabbar 组件有什么要求呢？首先，你得支持在选中特定带tabbar页面的情况下，tabbar中对应的tab有所变化(即从非选中状态变成选中状态，还要切换要对应的tab页面)，怎么支持呢？简单的方案，通过组件示例调用 setData 设置tab 的index，更进一步，可以专门封装一个方法，但不管怎么弄，这里本质上还是通过修改自定义组件的数据去影响自定义组件的渲染。

再者，自定义组件你得处于整个渲染页的最上层，不能被页面中的内容给盖住了，所以官方推荐的是使用 cover-view + cover-image 标签，但这里我们还是可以尝试下使用其他标签。然后设置下 i-index.

# seo
索引、爬虫

[sitemap 配置][https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/sitemap.html]
小程序根目录下的 sitemap.json 文件用于配置小程序及其页面是否允许被微信索引。
*如果没有 sitemap.json ，则默认为所有页面都允许被索引*

索引规则下面两个属性注意下

1. params
2. matching

本身索引规则用于判定指定页面是否允许被微信索引（就是被爬虫爬），然后设置了 params 和 matching，只有在页面带参数并且参数复合 matching 规定的规则时，才允许被微信索引。

比如下面这种，至于在 "path/to/page" 被打开时带参数a和b，并且只能有这两个参数时，才会被索引。

    {
      "rules":[{
        "action": "allow",
        "page": "path/to/page",
        "params": ["a", "b"],
        "matching": "exact"
      }, {
        "action": "disallow",
        "page": "path/to/page"
      }]
    }

这个规则还算有点用吧，一般情况下估计就是配置一下页面不需要被索引。


## 收录设置功能
页面收录设置：可对整个小程序的索引进行关闭（*此设置默认开启*），小程序管理后台-设置-基本设置-页面收录设置。



# 场景值
[场景值][https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/scene.html]用来描述用户进入小程序的路径。

## 获取场景值
对于小程序，可以在 App 的 onLaunch 和 onShow，或wx.getLaunchOptionsSync 中获取上述场景值





