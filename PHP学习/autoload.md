# 总览
总的来说，PHP Autoload 机制是为了解决在使用每一个 class 时，就要 include 或者 require 这个类的文件的这种情况，因为这样当我们要使用的 class 很多时，就会非常麻烦。

然后，PHP autoload 机制可以分为 3 个发展阶段。

# 第一阶段 \_\_autoload 函数

    void __autoload ( string $class )

该阶段的思路很简单，我们基于如上 \_\_autoload 的函数声明实现一个 \_\_autoload 函数；然后当 PHP 在执行当前文件时，发现当前文件定义了  \_\_autoload 函数，它就会在用到一个没有定义的类时，将这个类的名字作为参数传给 \_\_autoload 函数，而  \_\_autoload 函数要做的就是，基于传入的类名，找到这个类名对应的类文件的位置，然后在 include 或者 require 这个类文件。

所以，在这个阶段，PHP 所做的只是在执行一个文件时，如果遇到没有定义的类，就将这个类的名字作为参数传给 \_\_autoload 函数；至于 \_\_autoload 具体怎么实现，能不能根据传入的类名成功的 require 相应的类文件，PHP 是不关心的，这完全是我们要做的工作。

可以说，这个阶段的 Autoload 机制还是比较原始的，原始的地方在于 \_\_autoload 函数的实现并没有什么标准可以参考，全凭开发人员自己发挥。


# 第二阶段 Standard PHP Library
这个阶段相对于第一阶段，出现了两点变化：

1. 开发人员可以调用 spl_autoload_register 函数去注册一个或者多个__autoload 函数
    通过 spl_autoload_register 注册的这些函数，会扮演第一阶段 \_\_autoload 函数的角色，就是 PHP 在用到一个没有定义的类时，将这个类的名字作为参数传给它们。然后不变的是，这些函数的实现还是需要开发人员完成。

2. spl_autoload 函数

        void spl_autoload ( string $class_name [, string $file_extensions ] )

    上面我们说到，开发人员可以调用 spl_autoload_register 去注册一个或者多个__autoload 函数；但假如我们不使用任何参数调用 spl_autoload_register 函数呢？那这样其实我们也注册了一个   __autoload 函数，这个函数就是 spl_autoload，不管这个函数的默认实现对我们有没有用，但有总比没有好，不是吗。

OK，这就是第二阶段，相对于第一阶段，我们可以注册多个 \_\_autoload 函数，甚至还有一个默认的 \_\_autoload 实现。

嗯，还是有一点进步的。

# 第三阶段 PSR-0 与 PSR-4
so, what's this? PSR-0 与 PSR-4。

纵观第一阶段和第二阶段，PHP 都没有解决的一个问题就是 \_\_autoload 函数的实现没有任何标准可以依据，全凭开发人员自己发挥。但终于在第三阶段，标准来了，这就是 PSR-0 与 PSR-4，基于 PHP namespace 的两种标准。

下面是基于 PSR-0 标准的 \_\_autoload 函数实现：

        <?php

        function autoload($className)
        {
            $className = ltrim($className, '\\');
            $fileName  = '';
            $namespace = '';
            if ($lastNsPos = strrpos($className, '\\')) {
                $namespace = substr($className, 0, $lastNsPos);
                $className = substr($className, $lastNsPos + 1);
                $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
            }
            $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

            require $fileName;
        }
        spl_autoload_register('autoload');

从这段代码其实我们可以看出，PSR-0 其实是规定了类的命名空间与类的文件组织之间的关系，当我们遵守这种规定时，那简单的使用上面这个  \_\_autoload 函数或者对其稍作修改就可以了，在也不用开发人员自己去考虑  \_\_autoload 的实现了。可以说，从这里开始， PHP autoload 机制才是真正前进了一大步。

PSR-4 和  PSR-0 思路一样，然后也是 PSR-0 的继承者， PSR-0 已经不被推荐使用了。

## PSR-4: Autoloader
This PSR describes a specification for autoloading classes from file paths. 
This PSR also describes where to place files that will be autoloaded according to the specification.

关于 PSR-4 的详细规定，参考官方文档[《PSR-4: Autoloader》][2]吧。

# 参考文档
1. [PHP系列- Autoload 自动载入][1]

[1]: http://justericgg.logdown.com/posts/196891-php-series-autoload "PHP系列- Autoload 自动载入"
[2]: http://www.php-fig.org/psr/psr-4/ "PSR-4: Autoloader"
