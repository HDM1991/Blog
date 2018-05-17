# 用户认证
用户认证通常有哪些基本需求?


## 记住我具体指什么？
记住我这个功能，应该是将用户将认证用的 cookies 存储到客户端，然后后面可以通过 cookies 来确认用户是否已经登录。

如果不使用这个功能，用户在登录后，登录的状态是有时效的（通过 session 存储）。

# Laravel 用户认证
Laravel 的认证组件由 guards 和 providers 组成，Guard 定义了用户在每个请求中如何实现认证。

Provider 定义了如何从持久化存储中获取用户信息，Laravel 底层支持通过 Eloquent 和数据库查询构建器两种方式来获取用户，如果需要的话，你还可以定义额外的 Provider。

通过查看 *config/Auth.php* 文件中默认的 guard 配置，我发现一个 guard 由一个 driver 和一个 provider 组成。driver 用于在认证成功后管理这个认证状态，比如通过 session 管理，或者通过 token（本质上应该是 cookies） 管理；provider 用于在进行认证时获取数据库（通常用户认证信息在数据库中保存）中要进行认证的用户的信息。

一个 provider 由一个 driver 和一个 model 或者 table 组成，laravel 在 provider 中支持两种 driver：database 和 eloquent。当 driver 类型是 eloquent 时，一个 provider 就由一个 driver 和一个 model 组成；当 driver 类型是 database 时，一个 provider 就由一个 driver 和一个 table 组成。

其实整个验证逻辑都是围绕 guard 进行的，就是在做坐认证之前要指定一个 guard，或者使用默认的 guard。

就我们的需求来讲，只需要比葫芦画瓢增加几个 guard 就可以了。其他不需要。

另外，除了认证时这些最核心的概念，Laravel 也提供了一些现成的对 guard 进行调用、返回视图的支持，在 Illuminate\Foundation\Auth\AuthenticatesUsers、Illuminate\Foundation\Auth\RegistersUsers 这些 Trait 里面放。Laravel 的 pre-built authentication controllers 就是基于这些支持。

总的来说，Laravel 的用户认证用起来并不难，也好改。
