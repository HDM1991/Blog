# 前言
以前在公司做的一个项目中遇到的一个问题，还有意思的。
# 问题
大家有没有在 CakePHP 中使用过第三方库？我现在在引入了一个名为 [SimpleExcel](http://faisalman.github.io/simple-excel-php/api/0.3/) 的第三方库时遇到了一些问题。我觉得问题可能跟我把这个库的库文件放在了不正确的目录下有关。

正常情况下，该库在解压后，会有一个名为 SimpleExcel 的目录，该目录下的文件结构如下：

    ping@net:~/Desktop/faisalman-simple-excel-php-8d9fabc/src/SimpleExcel$ ls
    Exception  Parser  SimpleExcel.php  Writer

使用的话，只需要在相应的 PHP 文件中引入该库就可以了，如下所示：

    // test.php
    // 该文件和 SimpleExcel 目录在同一个目录，所以下面的 require_once 中的路径为 ./SimpleExcel/SimpleExcel.php
    use SimpleExcel\SimpleExcel;
    require_once('./SimpleExcel/SimpleExcel.php'); 

现在在 CakePHP 中，我是这样做的，将 SimpleExcel 目录放在了app/controllers 目录下，然后在相应的控制器中加入了如下代码：

    use SimpleExcel\SimpleExcel;
    require_once('SimpleExcel/SimpleExcel.php'); 

但在访问该控制器时，总是出现问题，错误信息如下：

    Warning (2): require_once(/opt/xplico/xi/app/controllers/SimpleExcel/ontroller.php) [function.require-once]: failed to open stream: No such file or directory [APP/controllers/SimpleExcel/SimpleExcel.php, line 43]

到底我哪里做错了？

# 答案
首先，请看下这篇文章《[PHP autoload机制详解](http://zccst.iteye.com/blog/1374737)》。因为之前遇到的问题的根本原因就在于 SimpleExcel 库在使用 autoload 机制时不够严谨。

现在，重新看下那个错误提示：

    Warning (2): require_once(/opt/xplico/xi/app/controllers/SimpleExcel/ontroller.php) [function.require-once]: failed to open stream: No such file or directory [APP/controllers/SimpleExcel/SimpleExcel.php, line 43]

从提示中可以看到，SimpleExcel.php 这个文件已经执行了，也就是说，之前我们将 SimpleExcel 库放在了 app/controllers 目录下，并在控制器中加入的如下代码是没有问题的。

    use SimpleExcel\SimpleExcel;
    require_once('SimpleExcel/SimpleExcel.php'); 

那为什么 SimpleExcel.php 会出现问题呢？现在定位到该文件出错的地方，如下所示：

    if (!class_exists('Composer\\Autoload\\ClassLoader', false)){
        // autoload all interfaces & classes
        spl_autoload_register(function($class_name){
            // TODO(H): 错误就出在这里的require_once语句上
            if($class_name != 'SimpleExcel') require_once(dirname(__FILE__).DIRECTORY_SEPARATOR.str_replace('\\', DIRECTORY_SEPARATOR, substr($class_name, strlen('SimpleExcel\\'))).'.php');
        });
    }

为什么这里会出错，结合上面的错误信息，这里 require_once 了一个不存在的文件？那这个不存在的文件时怎么来的呢？这就涉及到 autoload 机制。这个不存在的文件的路径是 SimpleExcel.php 文件自身所在目录加上当前控制器需要的类的名字构成的。作为一个控制器，它当然需要 Controller 父类，但这个类并不在 /opt/xplico/xi/app/controllers/SimpleExcel 目录下，当然就出错了。

其实，SimpleExcel 库的作者本意是利用 autoload 机制加载 SimpleExcel.php 自身所需要的一些类（位于Exception、Parser 和 Writer 目录下），但这里对要加载的类的过滤不够严谨，就出现了上面我们遇到问题。

修改方式，注释上面的代码，加入如下代码，解决问题。

    $currentDir = dirname(__FILE__);

    require_once($currentDir . '/Parser/IParser.php');
    require_once($currentDir . '/Parser/BaseParser.php');
    require_once($currentDir . '/Parser/CSVParser.php');
    require_once($currentDir . '/Parser/HTMLParser.php');
    require_once($currentDir . '/Parser/JSONParser.php');
    require_once($currentDir . '/Parser/TSVParser.php');
    require_once($currentDir . '/Parser/XLSXParser.php');
    require_once($currentDir . '/Parser/XMLParser.php');

    require_once($currentDir . '/Writer/IWriter.php');
    require_once($currentDir . '/Writer/BaseWriter.php');
    require_once($currentDir . '/Writer/CSVWriter.php');
    require_once($currentDir . '/Writer/HTMLWriter.php');
    require_once($currentDir . '/Writer/JSONWriter.php');
    require_once($currentDir . '/Writer/TSVWriter.php');
    require_once($currentDir . '/Writer/XLSXWriter.php');
    require_once($currentDir . '/Writer/XMLWriter.php');

    require_once($currentDir . '/Exception/SimpleExcelException.php');