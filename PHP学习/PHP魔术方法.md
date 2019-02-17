> PHP 将所有以 \_\_（两个下划线）开头的类方法保留为魔术方法。所以在定义类方法时，除了上述魔术方法，建议不要以 \_\_ 为前缀。


一些特殊的魔术方法需要注意下：

* \_\_construct
* \_\_destruct

# \_\_construct 
构造函数

# \_\_destruct
析构函数


## \_\_get、\_\_set
当调用当前环境下*未定义或不可见*的类属性时，就会调用这两个函数。

未定义指的就是要访问或者赋值的属性在类未定义；不可见指的是要访问或者赋值的属性在类中被定义为私有(private)，这两种情况后面统称为不可访问的属性。

可以通过这两个方法给类中所有的私有变量提供一个访问途径，不用再为每一个使用变量定义一个访问和赋值函数。如下所示：

    public function __get($property_name){
        if(isset($this->$property_name)){
            return($this->$property_name);
        }else{
            return(NULL);
        }
    }
        
    public function __set($property_name, $value){
        $this->$property_name = $value;

这也可以说是这两个函数最基础也是最常见的用法，也可以变化出其他用法。


在给不可访问属性赋值时，\_\_set() 会被调用。
读取不可访问属性的值时，\_\_get() 会被调用。


这里有一点注意一下，PHP 的类属性是不需要预先定义的。下面的代码会输出 "man"。

    class Test
    {
        public $name;
    }    
    $test = new Test;
    $test->sex = "man";
    echo $test->sex;

所以结合这点，\_\_get 和 \_\_set 就可以发挥出很有意思的作用.

# \_\_call、\_\_callStatic

在对象中调用一个不可访问方法时，\_\_call() 会被调用。
在静态上下文中调用一个不可访问方法时，\_\_callStatic() 会被调用。

可以利用这两个函数实现 C++ 中函数重载的概念，通过在 \_\_call 或者 \_\_callStatic() 中判断传入参数的类型，调用类中相应的方法。


[1]: http://php.net/manual/zh/language.oop5.decon.php#object.construct "构造函数和析构函数"
[2]: http://www.cnblogs.com/glory-jzx/archive/2012/05/23/2514173.html "PHP 魔术方法__set() __get() 方法"
[3]: https://blog.csdn.net/ebw123/article/details/41699031 "php __set __get __isset __unset用法防被忽悠分析"