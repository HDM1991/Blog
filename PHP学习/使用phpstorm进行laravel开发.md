# barryvdh/laravel-ide-helper
目前，掌握一些基础的用法就够了。

## 安装
两种安装方式。一般来讲，使用方式一就可以了

### 方式一
composer require barryvdh/laravel-ide-helper

After updating composer, add the service provider to the providers array in config/app.php

    Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class,

### 方式二，只在开发环境中使用 Laravel IDE Helper
composer require --dev barryvdh/laravel-ide-helper

add the following code to your app/Providers/AppServiceProvider.php file, within the register() method:

    public function register()
    {
        if ($this->app->environment() !== 'production') {
            $this->app->register(\Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class);
        }
        // ...
    }
This will allow your application to load the Laravel IDE Helper on non-production enviroments.

## 使用
### php artisan ide-helper:generate

You can configure your composer.json to do this after each commit:

    "scripts":{
        "post-update-cmd": [
            "Illuminate\\Foundation\\ComposerScripts::postUpdate",
            "php artisan ide-helper:generate",
            "php artisan ide-helper:meta"
        ]
    },

在 composer.json 中加入如上代码，每次提交就会运行里面的命令，保证 Laravel IDE Helper 生成的帮助文件总是最新的。
### php artisan ide-helper:models

By default, models in app/models are scanned.

    php artisan ide-helper:models --dir="path/to/models" --dir="app/src/Model"

默认情况下，这个命令会扫描 app/models 目录，可以通过 --dir 选项指定一个其他目录。

### php artisan ide-helper:meta


[1]: http://www.siguoya.name/pc/home/article/239#1.%E9%A6%96%E5%85%88%E6%98%AF%E6%8E%A7%E5%88%B6%E5%99%A8%E5%92%8C%E8%B7%AF%E7%94%B1%E7%9A%84%E4%BB%A3%E7%A0%81%E8%87%AA%E5%8A%A8%E5%AE%8C%E6%88%90%E4%B8%8E%E5%AE%9A%E4%BD%8D%E3%80%82 "使用PHPStorm来开发Laravel项目"
[2]: https://confluence.jetbrains.com/display/PhpStorm/Laravel+Development+using+PhpStorm#LaravelDevelopmentusingPhpStorm-3.GeneratethePHPDocHelperFileusingArtisan "Laravel Development using PhpStorm"