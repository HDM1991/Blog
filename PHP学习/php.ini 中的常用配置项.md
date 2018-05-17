# php.ini 中的常用配置项
## short_open_tag
决定是否允许使用 PHP 代码开始标志的缩写形式。

    <?
    
    ?>

## memory_limit
This sets the maximum amount of memory in bytes that a script is allowed to allocate.
脚本运行时允许分配的最大内存，默认为 128M。可以将其设置为 -1，表示不限制内存的分配。

## max_execution_time
脚本被解析器中止之前允许的最大执行时间，单位秒。默认为 30 秒；如果设置为0（零），没有时间方面的限制。


# ini_set
可以通过 ini_set 函数设置指定配置选项的值。这个选项会在脚本运行时保持新的值，并在脚本结束时恢复。

    string ini_set ( string $varname , string $newvalue )

# default_charset
*In PHP 5.6 onwards, "UTF-8" is the default value and its value is used as the default character encoding for htmlentities(), html_entity_decode() and htmlspecialchars() if the encoding parameter is omitted*. 

*The value of default_charset will also be used to set the default character set for iconv functions if the iconv.input_encoding, iconv.output_encoding and iconv.internal_encoding configuration options are unset*, and *for mbstring functions if the mbstring.http_input mbstring.http_output mbstring.internal_encoding configuration option is unset.*

All versions of PHP will use this value as the charset within the default Content-Type header sent by PHP if the header isn't overridden by a call to header().



[1]: http://php.net/manual/zh/ini.core.php#ini.short-open-tag "short_open_tag"
[2]: http://php.net/manual/zh/ini.core.php#ini.memory-limit "memory_limit"
[3]: [http://php.net/manual/zh/info.configuration.php#ini.max-execution-time] "max_execution_time"
[4]: http://php.net/manual/zh/function.ini-set.php "ini_set"


