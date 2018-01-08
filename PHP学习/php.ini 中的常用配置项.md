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



[1]: http://php.net/manual/zh/ini.core.php#ini.short-open-tag "short_open_tag"
[2]: http://php.net/manual/zh/ini.core.php#ini.memory-limit "memory_limit"
[3]: [http://php.net/manual/zh/info.configuration.php#ini.max-execution-time] "max_execution_time"
[4]: http://php.net/manual/zh/function.ini-set.php "ini_set"


