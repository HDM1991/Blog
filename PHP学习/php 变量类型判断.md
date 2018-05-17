# 总结
通常，使用 is_* 函数就可以了；但当我们需要判断对象的确切类型时，就需要使用 instanceof 或者 get_class。

# gettype
获取变量的类型

    string gettype ( mixed $var )

官方文档中不建议使用 [gettype][1] 判断类型，建议使用 is_* 函数代替。

* is_​array
* is_​bool
* is_​double
* is_​float
* is_​int
* is_​integer
* is_​long
* is_​null
* is_​numeric
* is_​object
* is_​real
* is_​resource
* is_​scalar
* is_​string


# is_​null
特殊的 [NULL][2] 值表示一个变量没有值。NULL 类型唯一可能的值就是 NULL。

在下列情况下一个变量被认为是 NULL：

* 被赋值为 NULL。
* 尚未被赋值。
* 被 unset()。

使用 (unset) $var 将一个变量转换为 null 将不会删除该变量或 unset 其值。仅是返回 NULL 值而已。

注意 (unset) $var 和 unset($var) 是两回事。

# get_class
返回对象的类名

    string get_class ([ object $object = NULL ] )

注意，如果 object 是命名空间中某个类的实例，则会返回带上命名空间的类名。


# instanceof
[instanceof][3] 用于确定一个 PHP 变量是否属于某一类 class 的实例：

[1]: http://php.net/manual/zh/function.gettype.php "gettype"
[2]: http://php.net/manual/zh/language.types.null.php#language.types.null.syntax "NULL"
[3]: http://php.net/manual/zh/language.operators.type.php "类型运算符
"