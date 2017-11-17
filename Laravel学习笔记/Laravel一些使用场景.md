# 在后台使用 blade 模板引擎获取渲染后的 html 代码
示例代码：

    public function construct_paper($data)
    {
        $v = view('offlinedocs.papers.paper', [
            'data' => $data,
            'offline' => $this
        ]);

        $html = $v->render();

        return $html;
    }

通常，view() 函数会返回一个 \Illuminate\View\View 实例，然后根据我们的需求调用影响的成员函数就可以了。

# blade模板 与 vue.js 语法冲突
下面是官方的说法：

由于许多 JavaScript 框架也使用「大括号」在浏览器中显示指定的表达式，因此可以使用 @ 符号来告知 Blade 渲染引擎该表达式应该维持原样。举个例子：

    <h1>Laravel</h1>

    Hello, @{{ name }}.

在这个例子中，@ 符号会被 Blade 移除。而且，Blade 引擎会保留 {{ name }} 表达式，如此一来便可跟其它 JavaScript 框架一起应用。

参考文档如下：
1. [Blade Templates][1]

# Laravel 怎么返回 json 数据，怎么设置 response 的 header 等信息
## 怎么返回 JSON 数据
The json method will automatically set the Content-Type header to application/json, as well as convert the given array into JSON using the json_encode PHP function:

    return response()->json(['name' => 'Abigail', 'state' => 'CA']);

If you would like to create a JSONP response, you may use the json method in addition to  setCallback:

    return response()
                ->json(['name' => 'Abigail', 'state' => 'CA'])
                ->setCallback($request->input('callback'));

## 怎么设置 response 的 header 等信息
1. 使用 response 函数
    下面是 response 函数的声明，怎么用，很清楚

    function response($content = '', $status = 200, array $headers = [])

2. 当我们不传入任何参数调用 response 函数时，response 函数会返回一个 \Illuminate\Contracts\Routing\ResponseFactory 实例，然后我们可以调用 \Illuminate\Contracts\Routing\ResponseFactory 实例给我们提供的各种成员函数获取特定类型的 Response 实例，这些成员函数都像上面 response 函数那样，支持传入 header 信息，下面是 view 成员函数的声明：

            public function view($view, $data = [], $status = 200, array $headers = []);

3. 调用 response 实例的 header 成员函数
    Laravel 给我们提供了很多种 response 实例对象，但我们都可以调用这个成员函数。这就是面向对象。

        $this header(string $key, string $value, bool $replace = true);

参考文档如下：
1. [HTTP Responses][2]

# 执行 php artisan migrate:rollback 出错
之前，我因为要修改 offline_docs 表中的一个字段的名字，需要执行如下命令：

    php artisan migrate:rollback

结果出现如下错误：

    [Illuminate\Database\QueryException]
    SQLSTATE[42S02]: Base table or view not found: 1051 Unknown table 'dtt.offlinedocs' (SQL: drop table `OfflineDocs`)

    [PDOException]
    SQLSTATE[42S02]: Base table or view not found: 1051 Unknown table 'dtt.offlinedocs'

然后我是如何解决的呢？我想既然它找不到这个表，那我就随便创建一个这个名字的表，字段什么的随意，结果还真就解决了这个问题。fuck.

参考文档如下：
1. [Database: Migrations][3]

[1]: https://laravel.com/docs/5.2/blade "Blade Templates"
[2]: https://laravel.com/docs/5.2/responses "HTTP Responses"
[3]: https://laravel.com/docs/5.2/migrations "Database: Migrations"
