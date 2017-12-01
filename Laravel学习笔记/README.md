# Installation
## Installing Laravel
Laravel utilizes Composer to manage its dependencies. So, before using Laravel, make sure you have Composer installed on your machine.
Laravel 使用 Composer 来管理它的包依赖。所以，我们需要先安装 Composer。

### Via Laravel Installer

    composer global require "laravel/installer" 

使用下面的方法吧。就不考虑在去掌握 laravel/installer 的用法了。
### Via Composer Create-Project

    composer create-project --prefer-dist laravel/laravel blog "5.2.*"

## Configuration
After installation, you should configure your application's document / web root to the public directory.

注意  application 的 web root 目录是 public 目录。

### Directory Permissions
After installing Laravel, you may need to configure some permissions. Directories within the *storage* and the *bootstrap/cache* directories should be writable by your web server or Laravel will not run.

然后 storage 和 bootstrap/cache 要被配置为可写。

### Application Key
当我们使用 Composer 创建 application 时，Composer 会帮我们将 .env.example 文件重命名为 .env；使用 Laravel Installer 创建目录，则需要我们手动去做这件事。

# Configuration
## Environment Configuration
All of the variables listed in this file will be loaded into the $\_ENV PHP super-global when your application receives a request.However, you may use the env helper to retrieve values from these variables in your configuration files.

后面这句话的意思是，在配置文件里，我们可以用 env helper 访问 .env 文件里面的配置项，下面就是一个例子。

    'debug' => env('APP_DEBUG', false),

Your .env file should not be committed to your application's source control, since each developer / server using your application could require a different environment configuration.

## Configuration Caching
To give your application a speed boost, you should cache all of your configuration files into a single file using the config:cache Artisan command. This will combine all of the configuration options for your application into a single file which will be loaded quickly by the framework.

You should typically run the *php artisan config:cache* command as part of your production deployment routine. The command should not be run during local development as configuration options will frequently need to be changed during the course of your application's development.

Configuration Caching 的意义在于提升 application 的速度表现（启动速度，相应速度，处理速度）。但是在 application 开发过程中，不推荐这样做，因为在这个过程中，我们可能要频繁的修改配置项。

然后，当从别人那里获取一份代码在本地部署时，一定要注意它之前有没有执行过 *php artisan config:cache* 。我昨天就因为这个耽误了很多时间，问题的原因是这个 application 之前执行过 *php artisan config:cache*，然后我不执行 *php artisan config:cache*  就直接访问 application，它就使用原来的配置项，就是我修改了配置文件也没有用。

## Maintenance Mode
维护模式，当我们需要对服务器上的 application 进行更新或者维护，就可以开启这个模式。的确实很有用的一个功能。

When your application is in maintenance mode, a custom view will be displayed for all requests into your application.

### Maintenance Mode Response Template
The default template for maintenance mode responses is located in  resources/views/errors/503.blade.php. You are free to modify this view as needed for your application.

在产品的开发中，我们基本是一定会修改 Maintenance Mode Response Template 的。

# Database
总体来说，Laravel 为我们连接和查询数据库提供了 3 中方式：raw SQL, the fluent query builder, and the Eloquent ORM.

## raw SQL
DB facade.

The DB facade provides methods for each type of query: select, update,  insert, delete, and statement.

## Creating Columns
Creating Columns 包含如下元素：
1. Available Column Types
2. Column Modifiers
    Column Modifiers 用于对列进行一些修饰，比如可以指定列的值唯一为NULL，指定列的默认值，为列指定注释，Set integer columns to UNSIGNED，还有就是指定列为索引(index modifiers)

## Database Migrations
Laravel's database migrations provide an easy way to define your database table structure and modifications using fluent, expressive PHP code.Instead of telling your team members to manually add columns to their local copy of the database, your teammates can simply run the migrations you push into source control.

 听起来挺有用的，但说实话，并没有什么实际感受。

感觉这个东西是这样的，就是一段用来创建数据库的 PHP 代码，和我们用 sql 语句去完成同样的工作差不多，不同的是这里我们用 php，然后还需要运行下面这条命令：
    
    php artisan migrate

那现在的问题就是，这种方式比我们用 sql 能完成的工作强在哪里，有什么工作是用这种方式可以完成，用 sql 语句完成不了的？

Laravel's schema builder
database schema



# Eloquent ORM
## Inserting & Updating Models

## Eloquent Models
Eloquent is Laravel's default ORM (object-relational mapper). Eloquent makes it painless to retrieve and store data in your database using clearly defined "models". Usually, each Eloquent model corresponds directly with a single database table.

so，Eloquent 就是 MVC 里面的 M 吗？

    php artisan make:model Task
    
We do not have to explicitly tell the Eloquent model which table it corresponds to because it will assume the database table is the plural form of the model name. So, in this case, the Task model is assumed to correspond with the tasks database table. 

好吧，好像所有的 framework 都就这一准则达成了一致，那现在这条准则也是我们认同的准则的一部分了。

Database Migrations 和 Eloquent Models 我觉得比使用 sql 去完成同样的工作强的一点是，不同的数据库时，它们的 sql 语法可能是稍有不同的，那这样，当我们要切换数据库时，使用 sql 就会是一件很麻烦的事情，但使用 Database Migrations 和 Eloquent Models 就不会出现这种问题。

然后，Eloquent Models 强大的另外一个地方是，当我们使用 Eloquent Models 为一个表创建一个 model 时，即使我们不为这个 model 添加任何功能，这个 model 自身提供的功能也已经很强大了，我们可以使用这个 model 做各种 sql 操作，查询、删除、更新等等，而且不用关心其背后的 sql 语句实现，也不用关心当我们切换数据库时，这些操作会因为不同数据库之间的不同而失败。

### Eloquent Relationships
这个就是我们在 sql 语句中定义的不同表之间的依赖关系，外键什么的。只不过这里是在 Laravel 的框架下以 PHP 代码的形式表达的，然后对于这种 relationship 的检查也从数据库引擎变成了 Laravel。我现在越发觉得，在Web开发中，我们并不需要直接以SQL语句的形式操作数据库了。




First, we will state that the name attribute on the model should be "mass-assignable". This will allow us to fill the name attribute when using Eloquent's create method:

    <?php

    namespace App;

    use Illuminate\Database\Eloquent\Model;
    
    class Task extends Model
    {
        /**
         * The attributes that are mass assignable.
         *
         * @var array
         */
        protected $fillable = ['name'];
    }

对上面这个把一个 attribute 声明为为 mass-assignable，我表示很是疑惑，这到底是什么意思。

什么是 mass-assignable，就是当我们在 laravel 中向数据库添加记录时，这个添加记录的操作可以分为两种：mass-assignable 和 非 mass-assignable。

什么是非 mass-assignable，比如下面这种：

    public function store(Request $request)
    {
        // Validate the request...

        $flight = new Flight;

        $flight->name = $request->name;
        $flight->email = $request->email;

        $flight->save();
    }

在上面的代码中，我们先是创建一个 Flight model 实例，然后在分别给该实例的 name 属性和 email 属性赋值，最后在调用 save 函数，将该记录插入数据库。

所谓非 mass-assignable，就是说上面这种对实例的属性一个一个进行复制的操作。

那什么是 mass-assignable 呢？就是与上面这种操作相反的情况。比如下面这种：

    $flight = App\Flight::create(['name' => 'Flight 10', 'email' => 'lionxyes@gmail.com']);

其实什么是  mass-assignable，更多是字面意思上的。

[Laravel - Mass Assignment][http://ju.outofmemory.cn/entry/136406]
[10.Laravel5学习笔记：Laravel中的批量赋值探索][http://blog.csdn.net/hel12he/article/details/47023253]

# Routing
Routes are used to point URLs to controllers or anonymous functions that should be executed when a user accesses a given page. By default, all Laravel routes are defined in the app/Http/routes.php file that is included in every new project.

Route，又是一个老生常谈的话题。

# Layouts & Views
Blade layouts，这是 Laravel 使用的 templating engine。

The .blade.php extension instructs the framework to use the Blade templating engine to render the view. 

# Validation

The ->withErrors($validator) call will flash the errors from the given validator instance into the session so that they can be accessed via the $errors variable in our view.

The $errors variable is available in every Laravel view. It will simply be an empty instance of ViewErrorBag if no validation errors are present.


    /**
     * Create a new task.
     *
     * @param  Request  $request
     * @return Response
     */
    public function store(Request $request)
    {
        $this->validate($request, [
            'name' => 'required|max:255',
        ]);

        // Create The Task...
    }
If you followed along with the basic quickstart, you'll notice this validation code looks quite a bit different! Since we are in a controller, we can leverage the convenience of the ValidatesRequests trait that is included in the base Laravel controller. This trait exposes a simple validate method which accepts a request and an array of validation rules.

We don't even have to manually determine if the validation failed or do manual redirection. If the validation fails for the given rules, the user will automatically be redirected back to where they came from and the errors will automatically be flashed to the session. Nice!
如此说来，确实挺方便的，既不用手动验证表单数据是否有效，也不需要在表单数据验证失败时，去手动做重定向，这个函数已经帮我们完成了这些。
然后这个函数在验证失败时也会调用 withInput 函数把用户的输入 flash 到 session 中，但是我们并不能简单的通过调用 

    session['name'];
去获取指定 name 的表单的用户输入，而是可以通过 old 函数获取

    old('name');
因为 laravel 并没有把用户的输入放到 session 的顶层。另外因为 session 包含的数据如此之多，所以我们直接去定位用户的输入在 session 中的位置非常麻烦，而且是及其不推荐的。所以 laravle 给我们提供了 old 帮助函数。

# middleware
这个概念我倒是第一次听说

# Dependency Injection
又是一个没有听说的概念。

all Laravel app folders are auto-loaded using the PSR-4 auto-loading standard,
什么意思？PSR-4 auto-loading standard 是什么？

Since Laravel uses the container to resolve all controllers, our dependencies will automatically be injected into the controller instance:
因为 Laravel 使用容器来解析所有的控制器，所以我们的依赖会自动被注入至控制器的实例中
这句话又是什么意思？

## service container
Laravel's service container is one of the most powerful features of the entire framework. After reading this quickstart, be sure to read over all of the container's documentation.

好吧，我会读的。

# Authorization
Laravel uses "policies" to organize authorization logic into simple, small classes. Typically, each policy corresponds to a model.

这TM又是什么？

# Errors & Logging
## Logging


# Collections
## Introduction
The Illuminate\Support\Collection class provides a fluent, convenient wrapper for working with arrays of data.

The results of Eloquent queries are always returned as Collection instances.

Remember, all of these methods may be chained to fluently manipulating the underlying array. Furthermore, almost every method returns a new Collection instance, allowing you to preserve the original copy of the collection when necessary:

需要记住的就这么多，Illuminate\Support\Collection 是对数组的一个封装，这样我们就不用使用函数去操作数组了，太原始了。然后，Illuminate\Support\Collection 的绝大多数方法都会返回一个全新的 Collection instance。

然后，我们就不一一去看每个方法了，就要看一些常用操作对应的方法。

1. count()
    The count method returns the total number of items in the collection. 从此告别 count 函数。
2. has()
    The has method determines if a given key exists in the collection. 这样我们就不用在使用 array_key_exists 这个函数了。
3. filter()
    The filter method filters the collection using the given callback, keeping only those items that pass a given truth test。从此告别 array_filter 函数。
4. implode()
    The implode method joins the items in a collection. 从此告别 implode() 函数，PHP 竟然把这个函数划分为 string 函数，也是醉了。

其他方法以后空闲时间可以看下。

## 字符串
很不幸，Laravel 并没有像上面通过在数组上封装一个 Illuminate\Support\Collection 类一样去增强 php 的字符串类型，Laravle 只是给我们额外提供了一些 php 本身不支持的函数，通过这种方式来增强字符串。虽然做不像它增强数组那样做的效果好，但好歹也算是加强了一下吧。

这些函数被 Laravel 称之为 Helper Functions，除了关于字符串的，还有一些其他种类的，具体可以看下如下文档：

[Helper Functions][1]

[1]:  https://laravel.com/docs/5.2/helpers "Helper Functions"



