# 概述
本篇博客以 python logging 模块的常见使用需求为主线讲解下 logging 模块的使用。

关于 python logging 模块的详细情况，可参考官方教程[《Logging HOWTO》][1]。

另外，关于 log 模块对项目的重要性及对 log 模块更深入的讲解，可以参考如下文章：

* [《 一个成熟的开发团队或开发者必备的工具系列之 Log 库》][2]
* [《深入理解log机制》][3]

# 需求一：临时使用，打印一些 log 到控制台或文件
在这种情况下，直接调用 logging 模块提供的如下函数即可：

* logging.debug
* logging.info
* logging.warning
* logging.error
* logging.critical

## 示例一：打印 Log 到控制台

    import logging

    logging.debug('debug level')
    logging.info('info level')
    logging.warning('warning level')
    logging.error('error level')
    logging.critical('critical level')

上述代码会在控制台输出如下内容：

    WARNING:root:warning level
    ERROR:root:error level
    CRITICAL:root:critical level

如上所示，对 logging.debug 和 logging.info 的调用没有输出任何信息。

因为这些函数实际上是使用了 logging 模块的 root logger，它们会调用 root logger 的同名函数，使用 root logger 的配置，如 level、handler、filter；另外当这些函数发现 root logger 当前没有任何 handler 时（默认情况下 root logger 没有 handler），它们会给 root logger 添加一个 handler，这个 handler 将 log message 输出到 sys.stderr.

因为 root logger 默认的 level 是 WARNING，所以示例中对于 logging.debug 和 logging.info 的调用没有输出任何信息；另外 root logger 默认也没有 handler，所以示例中 log 会打印到控制台。

因此，当需要改变调用上述函数的默认行为（如打印 log 到文件）时，就需要修改 root logger 的默认配置。通过 logging 模块的 [basicConfig][4] 函数，我们可以修改 root logger 的配置，如 level、handler、formatter 等。下面是对该函数的简单介绍，详细情况可以参考官方文档。

> Does basic configuration for the logging system by creating a StreamHandler with a default Formatter and adding it to the root logger.

另外 root logger 是 logging 模块整个 logger 树的根，正如它的名字表示的那样，root logger。

## 示例二：打印 log 到文件

    import logging
    logging.basicConfig(filename='example.log',level=logging.DEBUG)

    logging.debug('debug level')
    logging.info('info level')
    logging.warning('warning level')
    logging.error('error level')
    logging.critical('critical level')

上述代码会在名为 example.log 的文件中输出如下信息：

    DEBUG:root:debug level
    INFO:root:info level
    WARNING:root:warning level
    ERROR:root:error level
    CRITICAL:root:critical level

因为在上面对 basicConfig 函数的调用中，除了设置了 root logger 的 handler 外，还将 root logger 的 level 设置为 DEBUG，所以上述示例对 logging.debug 和 logging.info 的调用也会输出 log 到文件。

# 需求二：输出更详细的 log 信息
logging 模块在打印 log 时，除了输出我们指定的字符串外，还可以输出很多额外的信息，如文件名、行号，我们可以根据情况定制我们需要的信息。

下面是一个例子：

    formatters:
      detail:
        format: '%(pathname)s:%(lineno)d:%(levelname)s: %(message)s'

该示例用 YAML 配置了一个名为 detail 的 formatter，使用该 formatter 的 handler 输出的信息大致如下：

> test.py:9:CRITICAL: the message

关于这方面的详细信息可以参考[《15.7.7. LogRecord attributes》][5]。

# 需求三：基于 logging 模块构建项目的 log 系统
在这种情况下，需要考虑如下问题：

* 如何构建项目的 logger 树
* 如何配置 logging 模块

## 如何构建项目的 logger 树
logger 树的构建，具体需要根据实际情况而定。一种简单有效的方式是根据项目的的模块结构去构建对应的 logger 树，这种情况下项目的 logger 树和项目的模块结构保持一致。下面是[《Logging HOWTO》][1] 中对这种构建方式的详细介绍：

> A good convention to use when naming loggers is to use a module-level logger, in each module which uses logging, named as follows:
>
>     logger = logging.getLogger(__name__)
>
> This means that logger names track the package/module hierarchy, and it’s intuitively obvious where events are logged just from the logger name.

## 如何配置 logging 模块
配置 logging 模块，即对 logger 树中所有的的 logger、handler、filter、formatter 及它们之间的关系进行配置。

关于 logging 模块的配置，有如下三种方式：

1. Creating loggers, handlers, and formatters explicitly using Python code that calls the configuration methods listed above.
2. Creating a logging config file and reading it using the fileConfig() function.
3. Creating a dictionary of configuration information and passing it to the dictConfig() function.

实际使用时只需要掌握第三种方式就可以了，这种方式简单，灵活，可以满足各种需求。使用这种方式，我们需要创建一个 dictionary 描述当前项目要用到的所有 logger、handler、filter、formatter 的配置及它们之间的关系，然后把这个 dictionary 交给 dictConfig 函数去进行处理，之后我们只需要调用 logging.getLogger 获取相应的 logger 执行特定的操作就可以了。

关于如何配置这个 dictionary 的详细信息请参考[《"15.8.2. Configuration dictionary schema"》][6]。下面是我对该小节的总结。

# Configuration dictionary schema
关于 Configuration dictionary schema 需要关注的有如下几个点：

1. 语法
2. 如何使用自定义的 handler、formatter、filter
3. 如何引用外部对象
4. 如何引用内部对象

##  语法
语法这块很简单，没什么说的。

## 如何使用自定义的 handler、formatter、filter
使用自定义的 handler、formatter、filter 的关键点就是告诉 logging 怎么实例化对应的类，因为这些 handler、formatter、filter 对应的类都是我们自己定义的，logging 并不知道如何实例化。

logging 提供的解决方案如下：

> In order to provide complete flexibility for user-defined object instantiation, the user needs to provide a 'factory' - a callable which is called with a configuration dictionary and which returns the instantiated object. This is signalled by an absolute import path to the factory being made available under the special key '()'. Here's a concrete example:
>
>     formatters:
>       brief:
>         format: '%(message)s'
>       default:
>         format: '%(asctime)s %(levelname)-8s %(name)-15s %(message)s'
>         datefmt: '%Y-%m-%d %H:%M:%S'
>       custom:
>           (): my.package.customFormatterFactory
>           bar: baz
>           spam: 99.9
>           answer: 42

## 如何引用外部对象
外部对象指在当前 Configuration dictionary 中指定的 logger、handler、filter、formatter 对象之外的对象，如 sys.stderr。那当 Configuration dictionary 以文本文件的形式（如 JSON）的形式提供时，logging 就无法知道文件中指定的到底是一个字符串，还是一个对象。

 logging 提供的解决方案如下：

 > To facilitate this distinction, the configuration system looks for certain special prefixes in string values and treat them specially. For example, if the literal string 'ext://sys.stderr' is provided as a value in the configuration, then the ext:// will be stripped off and the remainder of the value processed using normal import mechanisms.

## 如何引用内部对象
内部对象即当前 Configuration dictionary 中指定的一些对象，如 logger、handler、filter、formatter。

虽然 logging 知道如何实例化这些对象，但是当 Configuration dictionary 中的一些自定义对象需要引用这些对象时，并且 Configuration dictionary 以文本文件的形式提供时，问题就来了，logging 不知道这些自定义对象要引用的是内部对象，还只是一个字符串？

logging 提供的解决方案如下：

> To cater for this, a generic resolution system allows the user to specify:
>
>     handlers:
>       file:
>         # configuration of file handler goes here
>
>       custom:
>         (): my.package.MyHandler
>         alternate: cfg://handlers.file
>
> The literal string 'cfg://handlers.file' will be resolved in an analogous way to strings with the ext:// prefix, but looking in the configuration itself rather than the import namespace.

## Else
关于 15.8.2.2. Incremental Configuration 和 15.8.2.7. Import resolution and custom importers 暂时不用关心。

[1]:    https://docs.python.org/2/howto/logging.html    "Logging HOWTO"
[2]:    http://blog.csdn.net/u013482618/article/details/46877111    " 一个成熟的开发团队或开发者必备的工具系列之 Log 库"
[3]:    http://feihu.me/blog/2014/insight-into-log/    "深入理解log机制"
[4]:    https://docs.python.org/2/library/logging.html#logging.basicConfig    "logging.basicConfig"
[5]:    https://docs.python.org/2/library/logging.html#logrecord-attributes    "15.7.7. LogRecord attributes"
[6]:    https://docs.python.org/2/library/logging.config.html#logging-config-dictschema    "15.8.2. Configuration dictionary schema"
