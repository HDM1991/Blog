这里谈一下 Laravel application 的 structure，或者说文件组织，比如 php 文件放在哪里，js、css 文件放在哪里等等。

总体来讲，Laravel 在这方面是给我们提供了参照的，比如 public 目录下放 js、css 文件、app\http\controllers 目录下存放控制器类，但是 Laravel 并没有强制我们一定要依据它提供的参照来组织文件。

但是，Laravel 给我们提供的参照还是挺好的，这里就谈一下。

# js css image 等前端资源文件放在哪里
Laravel 建议我们将这些文件放到 public 目录下，然后在 public 目录下我们可以自由对这些文件进行组织，比如我们可以创建一个 js 子目录，将所有的 js 文件都放到这个 js 子目录中；创建一个 css 子目录，把所有 css 文件都放到这个 css 目录中；创建一个 images 子目录，把所有图片文件都放到这个 images 子目录中。

可以说，怎么在 public 目录下对资源文件进行组织，是我们的自由，但是自由可以有，但自由后的另外一个问题需要解决，即使如何在 blade 模板中引用这些资源文件。

这个问题本质上就是在 blade 模板中，构建这些资源的路径的问题，我们可以构造一个完整的 url，比如下面这样：

    <script type="text/javascript" src="{{ URL::asset('public/js/ueditor1_4_3_3-utf8-php/ueditor.config.js') }}"></script>

上面我们使用 URL::asset 函数构造了一个完整的 url，在我的配置环境下，生成的 url 如下：

    <script type="text/javascript" src="http://localhost:5789/app/public/js/ueditor1_4_3_3-utf8-php/ueditor.config.js"></script>

也可以提供一个相对路径，比如下面这样：

        <link rel="stylesheet" type="text/css" href="public/css/style.css">


但不管时提供完整的 URL，还是提供一个相对路径，我们都需要搞清楚的一个问题是，当前路径是什么？因为不管你是使用 URL::asset 函数构造完整路径或者是使用相对路径，都是基于当前路径的。

Note: Laravel 推荐 configure your application's document / web root to the public directory。在上面的实例中，我们在路径中指定了 public 目录，这是因为我没有照 laravel 推荐的那样做，这是一种不好的示范，大家不要模仿。

[1]: https://laravel.io/forum/09-17-2014-problem-asset-not-point-to-public-folder "[PROBLEM] asset() not point to public folder"
[2]: https://stackoverflow.com/questions/14837065/how-to-get-public-directory "How to get public directory?"


