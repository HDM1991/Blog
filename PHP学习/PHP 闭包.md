匿名函数（Anonymous functions），也叫闭包函数（closures），允许 临时创建一个没有指定名称的函数。

匿名函数目前是通过 Closure 类来实现的。

闭包可以从父作用域中继承变量。 任何此类变量都应该用 use 语言结构传递进去。 PHP 7.1 起，不能传入此类变量： superglobals、 $this 或者和参数重名。闭包的父作用域是定义该闭包的函数。

NOTE: 通过 use 传递给觅名函数的变量的值，在定义觅名函数时已经确定，即在定义觅名函数后，在父作用域中改变通过 use 传递的变量的值，是不会影响到觅名函数中这个变量的值的。示例如下：

    // 继承 $message
    $message = 'hello';
    $example = function () use ($message) {
        var_dump($message);
    };
    echo $example();

    // Inherited variable's value is from when the function
    // is defined, not when called
    $message = 'world';
    echo $example();

上面示例代码的输出如下：

    string(5) "hello" string(5) "hello"

可以通过引用的方式改变上述行为，示例如下

    // Inherit by-reference
    $message = 'hello';
    $example = function () use (&$w) {
        var_dump($message);
    };
    echo $example();

    // The changed value in the parent scope
    // is reflected inside the function call
    $message = 'world';
    echo $example(); 

上述示例的输入如下：

    string(5) "hello" string(5) "world"

As of PHP 5.4.0, when declared in the context of a class, the current class is automatically bound to it, making $this available inside of the function's scope.
从 PHP 5.4 开始，当在一个类中定义觅名函数时，可以直接在觅名函数中通过 $this 访问当前类实例。

    <?php
    class Test
    {
        public function testing()
        {
            return function() {
                var_dump($this);
            };
        }

        public $name = "test";
    }

    $object = new Test;
    $function = $object->testing();
    $function();
    ?>


# Static anonymous functions
As of PHP 5.4, anonymous functions may be declared statically. This prevents them from having the current class automatically bound to them. Objects may also not be bound to them at runtime.

静态觅名函数从 PHP 5.4 引入，当在类中定义静态觅名函数时，当前类实例不会在自动通过 $this 传递给静态觅名函数。也无法将一个静态觅名函数绑定到一个类实例。


# 总结
总结来讲，主要需要注意的就是如下点：

1. 父作用域变量的传递，需要用 use
2. 父作用域通过 use 传递给命名函数的值，在觅名函数定义时确定
3. 在类中定义觅名函数时，当前类实例自动通过 $this 传递给觅名函数。

其他点，如通过 use 传递变量的引用，静态觅名函数暂时不用关心。 

[1]: http://php.net/manual/zh/functions.anonymous.php "匿名函数"