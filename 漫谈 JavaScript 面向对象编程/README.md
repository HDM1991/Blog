# 前言
为什么要写这篇博客，JavaScript 面向对象编程有什么值得说的吗？

作为一个以 C/C++ 作为主要开发语言，也是自身最熟悉的语言的程序员，JavaScript 面向对象编程真的有很多值得说的（吐槽的）。在最开始接触 JavaScript 面向对象编程时，它真的让我很疑惑，诡异的语法，让人摸不着头脑的 this，都让人抓狂，虽然可以比葫芦画瓢，但内心的疑问却一直存在，而且与日俱增，深入学习之后，才发现 JavaScript 面向对象编程和 C++ 是有多么大的不同。

这篇博客，一方面是给出一个对 JavaScript 面向对象编程进行学习的主线，方便要对 JavaScript 面向对象编程进行学习的同学；但更多的还是讲述自己对于 JavaScript 基于原型的面向对象的理解，并探讨其存在的问题。

另外，关于 JavaScript 面向对象编程的教程方面，推荐《全面理解面向对象的 JavaScript》这篇文章。这是一篇非常好的文章，教程。下面我也会多次引用这篇文章中的内容。

# 综述
在学习具体东西之前，首先要明确一个问题：JavaScript 是面向对象语言吗？答案是是，而且 JavaScript 跟 C++、Java 相比是一种更彻底的面向对象语言，在JavaScript 中的所有事物都是对象，包括函数。那 JavaScript 和 C++、Java 在面向对象方面相比，有什么不同吗？简单来说，就是 JavaScript 中并没有类的概念，JavaScript 的世界只有对象。我相信，这会让所有习惯了 C++、Java 的程序员感到疑惑，没有类怎么创建对象，没有类还能进行面向对象编程吗？下面我们就回答下这个问题。

首先，我们要知道，面向对象只是一种编程思想，而 C++、Java 中的类只是实现面向对象编程思想的一种方式，这种方式方便了用户进行面向对象编程；但这并不意味着类是实现面向对象编程思想的唯一方式。没有类，我们依然可以通过其他方式进行面向对象编程，而 JavaScript 采用的就是一种名为基于原型的面向对象方式，一种和类完全不同的面向对象实现方式。所以在学习 JavaScript 的面向对象编程时，最好忘掉 C++、Java 中和类相关的那些东西。

下面我们就来看看 JavaScript 给我们提供了什么，我们能用 JavaScript 提供给我的东西做什么？

# JavaScript 给我们提供了什么
这块主要是说下 JavaScript 语言中和面向对象编程息息相关的的一些语义和语法规定。

## this
首先，我们需要了解的是 JavaScript 中的 this。这方面的资料如下：

* [深入浅出 JavaScript 中的 this][1]

## JavaScript 中创建对象的方式
JavaScript 提供了如下两种方式创建对象：

1. 定义并创建对象的实例
2. 使用函数构造器构造对象

关于这两种方式的详细信息，大家可以参考下面的这些资料：

* [JavaScript 对象][2]
* [全面理解面向对象的 JavaScript][3]

这里我主要说下自己对使用函数构造器构造对象这种方式的理解。下面是通过这种方式创建对象的一个简单例子：

     // 构造器 Person 本身是一个函数
     function Person() { 
         this.name = "小明";
         this.sex = "男";
     } 
     // 使用 new 关键字构造对象
     var p = new Person();

首先，说下 new 这个关键字。正如[《全面理解面向对象的 JavaScript》][3]这篇文章中提到的，JavaScript 中的 new 和 C++、Java 中的 new 并没有任何关系，所以这里看到 new 也不要有什么疑惑，不要把它和 C++、Java 中的 new 联系起来。

那如何来理解下面这行代码呢？

    // 使用 new 关键字构造对象
    var p = new Person();

我觉得虽然不太严谨，但是可以这样理解：

1. JavaScript 创建一个属性为空的对象

        var p = {};

2. JavaScript 设置这个对象的一些默认属性

        p.constructor = Person;
        p.__proto__ = Person.prototype;
        ...

3. JavaScript 通过这个对象调用 Person 函数
        
        p.Person();
    
    这时 Person 函数中的 this 绑定到对象 p 上。下面的代码也就相当于给 p 添加了属性 name 和 sex。

        this.name = "小明";
        this.sex = "男";

## 函数的 prototype 属性
函数的 prototype 属性，即我们上面提到的原型，所谓 JavaScript 基于原型的面向对象，其实就是基于函数的 prototype 属性，关于函数的 prototype 属性，[《全面理解面向对象的 JavaScript》][3]这篇文章已经讲的非常好了，所以这里我只是做下总结：

1. 函数都拥有一个名为 prototype 属性，该属性默认指向一个属性为空的 Object 对象，但可以手动设置该属性的值，让它指向其他对象
2. 对象都拥有一个名为 \__proto__ 的隐藏属性，该属性指向用于创建该对象的函数的 prototype 属性所指向的那个对象；也就是说，对象的 \__proto__  属性和用于创建该对象的函数的 prototype 属性，都指向同一个对象
3. 结合 1、2，一个对象的 \__proto__ 属性指向用于创建该对象的函数的 prototype 属性所指向的对象，然后这个对象的 \__proto__ 又指向用于该对象的函数的prototype 属性所指向的对象。如此一来，对于每个对象，我们就会得到相应的一个对象链，称之为原型链。原型链的第一个对象即为对象本身，原型链中其他对象则都是前一个对象的构造函数的 prototype  属性所指向的对象，当然这个链会有一个结束，因为 Object.prototye.\__proto__ 为 null
4. 对于一个对象，当要获取该对象的某个属性的值时，如果该对象并没有该属性，但是该对象的原型链中的其他对象有该属性，JavaScript 会返回该对象原型链中第一个具有该属性的对象的该属性的值；但注意，当设置一个对象的某个属性的值时，如果该对象并没有该属性，但该对象的原型链中的其他对象有这个属性，JavaScript 的行为；JavaScript 会给该对象添加该属性，然后再给该对象的这个属性赋值，而不是给该对象原型链中首先具有该属性的对象的该属性赋值。这点不难理解，因为通过一个函数创建的所有对象的 \__proto__ 所指向的都是同一个对象，进一步讲，这些对象的原型链除去第一个对象(即它们自身)外，其他对象都是一样的，如果当我们设置一个对象自身不存在而它的原型链中的其他对象存在的属性时，JavaScript 的行为不是像上面那样，那通过这个函数创建的所有对象都会受到影响。

OK，上面就是 JavaScript 提供给我们的进行面向对象编程的所有东西，但我们真的可以用这些东西进行面向对象编程吗？答案是能。下面我们就来看一下。

# 我们能用 JavaScript 给我们提供的东西做什么
首先，我们先举一个简单的例子，这个例子完全基于上面提到的东西创建，大家可以通过这个例子感受下 JavaScript 原汁原味的基于原型的面向对象是什么样子，思考下它的优缺点。

    // 声明 Animal 对象构造器
    function Animal() {
       this.name = "animal";
       this.weight = 0;
       this.eat = function() { 
           alert( "Animal is eating!" ); 
       }
    } 

    // 声明 Mammal 对象构造器
    function Mammal() { 
       this.name = "mammal"; 
       this.sex = "male";
    } 
    // 指定 Mammal 对象的原型为一个 Animal 对象。
    // 实际上此处便是在创建 Mammal 对象和 Animal 对象之间的原型链
    Mammal.prototype = new Animal(); 

    // 声明 Horse 对象构造器
    function Horse( height, weight ) { 
       this.name = "horse"; 
       this.height = height; 
       this.weight = weight; 
    } 
    // 将 Horse 对象的原型指定为一个 Mamal 对象，继续构建 Horse 与 Mammal 之间的原型链
    Horse.prototype = new Mammal(); 

    // 重新指定 eat 方法 , 此方法将覆盖从 Animal 原型继承过来的 eat 方法
    Horse.prototype.eat = function() { 
       alert( "Horse is eating grass!" ); 
    };
    
    // 利用 Horse 函数，创建一个对象
    var horse = new Horse( 100, 300 ); 

在这个例子中，我们创建了3个函数，分别为 Animal、Mammal 和 Horse，我们分别可以用它们创建代表动物的 Animal 对象、代表哺乳动物的 Mammle 对象和代表马的 Horse 对象，那怎么去理解这 3 种对象之间的继承关系呢？这里，我们通过如下几种操作说明。

1. 我们要给 horse 的 sex 属性赋值

        horse.sex = "female";

    有问题吗？没有问题。horse 对象本身没有 sex 属性，但 JavaScript 会给 horse 添加一个 sex 属性，然后给 horse 的 sex 属性赋值，即使 horse 对象的原型链中的 Mammal 对象有这个属性。为什么这样做？原因我们上面已经说过了，通过 Horse 函数创建的对象，它们的原型链中的 Mammal 对象是同一个，如果不这么做，就会出现修改了一个 Horse 对象的 sex 值，其他所有 Hores 对象的值都会受影响的情况。
2. 我们要访问 horse 的 sex 属性

        console.log(horse.sex);
    
    有问题吗？没有问题。如果之前我们对 horse 的 sex 属性赋过值，那这里 JavaScript 读取的就是 horse 的 sex 属性的值；如果没有，虽然 horse 对象本身没有 sex 属性，但 horse 对象的原型链中的 Mammal 对象有这个属性，JavaScript 会给我们返回 Mammal 对象的 sex 属性的值。总之，JavaScript 总是返回你想要的值。

3. 我们要访问 eat 方法
    
    horse.eat();

    同样没有问题，eat 也是一个属性，只不过这个属性的值是一个函数，按照我们上面提到的原则，下面这个函数会执行，这正是我们想要的。

        function() { 
               alert( "Horse is eating grass!" ); 
            };

如上所属，上面 3 种操作都没有问题，这说明基于原型的面向对象确实是一种有效的面向对象编程方式。但不得不说 JavaScript 基于原型的面向对象和 C++、Java的基于类的面向对象完全不同。如果按照 C++、Java 的逻辑，每个 Hores 对象会拥有自己的 Mammle 对象和 Animal 对象，当我们要给 sex 属性赋值时，是给这个对象的自己的 Mammle 对象的 sex 属性复制，当我们要读取 sex 属性，读的也是这个对象的自己的 Mammle 对象的 sex 属性。而 JavaScript 的所有 Horse 对象都拥有相同的 Mammle 对象、Animal 对象，但如上所属，这并没有影响到我们进行的操作。总之，虽然会让人感到奇怪，但的确可以正常工作。

但是，真的没有问题吗？正如 [《全面理解面向对象的 JavaScript》][3]提到的，基于原型的继承方式，虽然实现了代码复用，但其行文松散且不够流畅，可阅读性差什么什么的...而类式继承方式又有什么什么优点，所以目前一些主流的 JS 框架都提供了类式声明方法（语法上看上去类似 C++、Java，但实际上还是基于原型的继承），方便用户进行面向对象编程，

但我觉得出现这种情况最主要的原因是，JavaScript 自身规定的东西太少了，这种少可以称之为灵活，但我觉得更多是带来一种混乱和不便，特别是对于普通程序员。下面我们就几个方面具体讨论下这种混乱和不便。

1. 对象的方法怎么定义
    在上面的例子中，我们为 Animal 对象和 Horse 对象都定义了 eat 方法，但注意，Animal 对象的 eat 方法，是对象自身的一个属性；而 Horse 对象的 eat 方法却是 Horse.prototyte 所指向的的对象（一个 Mammle 对象）的一个属性。完全不同的两种方式，不是吗！但事实是，无论通过上述那一种方式定义对象的方法，这个方法都是可以被正常调用的。但这两种方式真的等价吗？不。第一种方式会导致所有通过该函数创建的对象都有一个自己的 eat 方法，这就存在一个浪费内存的问题；而第二种方法，虽然不会出现这种问题，通过该函数创建的对象拥有的是同一个 eat 方法，但你不觉得这种方式也很奇怪吗？我们明明定义的是 Horse 对象的 eat 方法，为什么这个方法变成了一个 Mammle 对象的属性。所以，对象的方法到底该怎么定义，到底该采用哪种方法？对不起，JavaScript 没有规定。

2. 属性的继承
    以在上面的例子中 sex 属性为例， Mammle 对象拥有 sex 属性，Horse 对象继承 Mammle 对象，那在上面的例子中，sex 属性的继承是如何处理的呢？首先，所有的 Horse 对象创建之初是没有 sex 属性的，这时如果读取的 Horse 对象的 sex 属性，实际上读取的是 Horese 对象原型链中 Mammle 对象中的 sex 属性；然后，在我们对一个 Horse 对象的 sex 属性赋值时，一个 Horse 对象才会拥有自己的 sex 对象，在这之后，无论是读取还是赋值，访问的都是 Horse 对象自己的 sex 属性。虽然可以工作，但这种处理仍然很诡异。
    
    这里我们不拿 C++、Java 中的属性的继承做对比，我们来看看上面的例子如果按照 John Resig 实现的 [Simple Inheritance][4] （一种类式声明方法）的逻辑编写，sex 属性的继承是怎么处理的？首先所有的 Horse 对象在创建之初，就会拥有自己的 sex 属性，这样，从这个对象被创建出来，所有的读取和赋值，操作的都是 Horse 对象自己的 sex 属性；其次虽然所有的 Horse 仍然会拥有同一个 Mammle 对象，但这会是一个属性为空的 Mammle 对象，这样就不会出现上面的例子中出现的 Horse 对象有一个 sex 属性，然后这个 Horse 对象的原型链中的 Mammle 对象也有一个 sex 属性的情况。是不是相对上面的例子中的处理清晰了很多。

总之，说了这么多，我觉得为 JavaScript 提供类式声明方法的确是非常有必要的，这不仅仅是为了让代码的可读性更好，另一方面也是进一步规范一些行为的处理，因为 JavaScript 规定的的确有点少。

OK，上面一直在说为 JavaScript 提供类式声明方法，这里我们就简单的说下其中一种，就是上面已经提到的由 jQuery 之父 John Resig 在搏众家之长之后，用不到 30 行代码便实现的 [Simple Inheritance][4]。但在这里我们只是说说这个模型对一些行为的处理，关于这种方式的语法、实现原理，暂时就不说了，有兴趣的同学可以自己调试下，并不难理解。

1. 对象的方法如何定义
    在这种模型下，对象的方法均定义为用于创建该对象的函数的 prototype 属性所指向的对象的属性，上面已经提到这种做法的优点，虽然从语义上看起来挺奇怪的。
2. 属性的继承
    在这种模型下，一个对象在创建时就会拥有自己的，原本是原型链中其他对象拥有的属性；其次，这个对象的原型链中除了自身之外的其他对象，都会是一个属性为空的对象（注意这里的属性不包括在 1 中为一个空对象添加的值为函数的属性）

        <script type="text/javascript">
          /* Simple JavaScript Inheritance
           * By John Resig http://ejohn.org/
           * MIT Licensed.
           */
          // Inspired by base2 and Prototype
          (function(){
            var initializing = false, 
        
            fnTest = /xyz/.test(function(){xyz;}) ? /\b_super\b/ : /.*/;
           
            // The base Class implementation (does nothing)
            this.Class = function(){};
           
            // Create a new Class that inherits from this class
            Class.extend = function(prop) {
              var _super = this.prototype;
             
              // Instantiate a base class (but only create the instance,
              // don't run the init constructor)
              initializing = true;
              // 创建一个父类的对象，因为 initializing = true，所以这其实是一个空对象
              var prototype = new this();
              initializing = false;
             
              // Copy the properties over onto the new prototype
              for (var name in prop) {
                // Check if we're overwriting an existing function
                var v1 = typeof prop[name] == "function";
                var v2 = typeof _super[name] == "function";
                var v3 = fnTest.test(prop[name]);
                var v4 = function(name, fn) {
        
                    // 下面返回的这个函数如何理解？我觉得主要由下面几点：
                    // 1. 这个函数是被怎样调用的，这决定这这个函数中的 this 如何解释
                    //    
                    // 2. 对这个函数中的变量 _super 的理解，注意是 _super，不是 this._super。
                    //    这里设计到闭包的概念。
                    // 
                    // 这个函数的调用方式是，通过该类创建的对象调用，所以这个 this 就代表这个类，然后 _super 代表父类的原型
                    // 于是下面函数的逻辑就是给对象添加一个 _super 属性，该属性的值为父类的原型中的同名函数，考虑到对象中该函数的实现，
                    // 如下面的Programmer 类的 init 函数，就实现了在调用之类 init 函数时，先调用父类中的同名函数，然后在执行之类函数中的内容
                    // 主要是用在 init 函数，init 函数我们可以看做 C++ 中类的构造函数，另外 init 中的 this._super() 也是不能少的。
                    // 
                    return function() {
                      var tmp = this._super;
                     
                      // Add a new ._super() method that is the same method
                      // but on the super-class
                      this._super = _super[name];
                     
                      // The method only need to be bound temporarily, so we
                      // remove it when we're done executing
                      var ret = fn.apply(this, arguments);        
                      this._super = tmp;
                     
                      return ret;
                    };
                  };
        
                prototype[name] = v1 && v2 && v3 ? v4(name, prop[name]) : prop[name];
              }
              
              // 下面 Person 和 Programmer 其实都是这个函数
              // The dummy class constructor
              function Class() {
                // All construction is actually done in the init method
                if ( !initializing && this.init )
                  this.init.apply(this, arguments);
              }
              
              // prototype 是父类的一个对象
              // Populate our constructed prototype object
              Class.prototype = prototype;
             
              // 
              // Enforce the constructor to be what we expect
              Class.prototype.constructor = Class;
           
              // And make this class extendable
              Class.extend = arguments.callee;
             
              return Class;
            };
          })();
        </script>


# 总结
JavaScript 的基于原型的面向对象和基于类的面向对象确实是完全不同的东西，我不敢随便评价那种方式更好。但从一个普通开发者的角度，一个语言使用者的角度讲，我更喜欢基于类的面向对象。JavaScript 的基于原型的面向对象让我感到更多是不便和混乱，而不是灵活。如果 JavaScript 自身就拥有类式声明方式，我觉得我会改变我的看法，但问题是它没有，这就意味着，作为一个普通的开发者，你要么自己去实现一些东西，定义一些规则，要么去引入并学习一个第三方方案，你觉得这样方便吗？

[1]:    http://www.ibm.com/developerworks/cn/web/1207_wangqf_jsthis/    "深入浅出 JavaScript 中的 this"
[2]:    http://www.w3school.com.cn/js/js_objects.asp    "JavaScript 对象"
[3]:    http://www.ibm.com/developerworks/cn/web/1304_zengyz_jsoo/   "全面理解面向对象的 JavaScript"
[4]:    http://ejohn.org/blog/simple-javascript-inheritance/    "Simple JavaScript Inheritance"