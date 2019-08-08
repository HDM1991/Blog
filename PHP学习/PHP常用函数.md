# iconv
out_charset 参数后可以添加字符串 //TRANSLIT 或者 //IGNORE，基本上 //IGNORE 是一定要加的，//TRANSLIT 并没有什么用。


# empty isset 
这两个函数的区别


# http_build_query
    string http_build_query ( mixed $query_data [, string $numeric_prefix [, string $arg_separator [, int $enc_type = PHP_QUERY_RFC1738 ]]] )

numeric_prefix, 如果数组某个值的键是数字，这个参数会起作用；不是数字，即使传入该参数，也不会有用。

# gzdecode

# set_time_limit

> The set_time_limit() function and the configuration directive max_execution_time only affect the execution time of the script itself. Any time spent on activity that happens outside the execution of the script such as system calls using system(), stream operations, database queries, etc. is not included when determining the maximum time that the script has been running. This is not true on Windows where the measured time is real.



[1]: http://php.net/manual/zh/function.iconv.php "iconv"

[2]: http://php.net/manual/zh/function.empty.php "empty"

[3]: http://php.net/manual/zh/function.http-build-query.php "http_build_query"
[4]: http://php.net/manual/zh/function.set-time-limit.php "set_time_limit"