# class dependencies
什么是类依赖？在一个类 A 中要用到另外一个类 B，那么 A 就依赖 B。

下面是一个例子。在这个例子中，Foo 类在 Foo::doSomething 方法中要用到类 Bar，所以 Foo 类就依赖类 Bar。

    // 示例一
    class Bim
    {
        public function doSomething()
        {
            echo __METHOD__, '|';
        }
    }

    class Bar
    {
        public function doSomething()
        {
            $bim = new Bim();
            $bim->doSomething();
            echo __METHOD__, '|';
        }
    }

    class Foo
    {
        public function doSomething()
        {
            $bar = new Bar();
            $bar->doSomething();
            echo __METHOD__;
        }
    }

    $foo = new Foo();
    $foo->doSomething(); //Bim::doSomething|Bar::doSomething|Foo::doSomething



# dependency injection
什么是依赖注入？在上面的示例一中，Foo 类依赖 Bar 类，于是在自己的 Foo::doSomething 执行如下代码实例化了一个 Bar 类。

    $bar = new Bar();

把 Bar 类的实例化变成像示例二中这样，就叫依赖注入。结合示例一和示例二，依赖注入可以这样理解，当一个类 A 依赖另外一个类 B 时，类 A 不再自己实例化类 B（像示例一种那样），而是像示例二中那样，在构造函数中接受一个类 B 的实例，也就是说，类 B 的实例化在类 A 外部完成，完成后再把这个实例交给类 A，这就叫注入。

    // 示例二
    class Bim
    {
        public function doSomething()
        {
            echo __METHOD__, '|';
        }
    }

    class Bar
    {
        private $bim;

        public function __construct(Bim $bim)
        {
            $this->bim = $bim;
        }

        public function doSomething()
        {
            $this->bim->doSomething();
            echo __METHOD__, '|';
        }
    }

    class Foo
    {
        private $bar;

        public function __construct(Bar $bar)
        {
            $this->bar = $bar;
        }

        public function doSomething()
        {
            $this->bar->doSomething();
            echo __METHOD__;
        }
    }

    $foo = new Foo(new Bar(new Bim()));
    $foo->doSomething(); // Bim::doSomething|Bar::doSomething|Foo::doSomething

# 控制反转
首先，这个控制指的控制什么？控制类之间的依赖关系。那什么是控制反转？

结合上面两个例子。示例一中，类 Foo 依赖类 Bar，这种依赖关系是由类 Foo 控制的，如果类 Foo 想使用另外一个类替换类 Bar，那就需要修改类 Foo 的代码；示例二中，类 Foo 对类 Bar 的依赖关系由外部控制，结合面向对象中接口的概念，加入现在有两个类 Memcache、Redis 都实现了接口 IMemCache，然后类 Foo 依赖 Memcache 或者 Redis，那使用如下代码，Foo 是可以在对 Memcache 和 Redis 之间的依赖无缝切换的。
    
    // 示例三
    class Foo
    {
        private $mem_cache;

        public function __construct(IMemCache $mem_cache)
        {
            $this->mem_cache = $mem_cache;
        }
    }

那控制反转我觉得可以这样理解，当一个类 A 依赖于类 B，当类 A 对类 B 的这种依赖关系不是由类 A 决定，而是由使用类 A 的外部决定的话，就叫控制反转。类 A 对类 B 的依赖关系由类 A 决定反转到了使用类 A 的外部的手里。

# 服务容器

[1]: https://segmentfault.com/a/1190000002424023 "PHP程序员如何理解依赖注入容器(dependency injection container)"