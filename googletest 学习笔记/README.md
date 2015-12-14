# 前言
googletest 是由 Google 开发的开源 C++ 单元测试框架，在很多开源项目中（如 chromium）都有使用。

这篇博客记录在了自己在学习 googletest 的文档[《Primer》][2]和[《AdvancedGuide》][3]时做的一些笔记，主要是自己对 googletest 的一些特性的理解和总结。

正如在[《漫谈单元测试》][4]这篇博客中所说，googletest 的核心就是一些判断真假的宏，至于 googletest 提供的其他特性，如 Test Fixtures之类，只是它为我们提供的在使用这些宏时遇到的一些常见需求（如，不同 Test 之间共享资源）的通用解决方案。

所以 googletest 的学习并不复杂，但真正想用好单元测试，这只是一个开始。

# 笔记
## Basic Concepts
一些基本概念

* assertion
* test
* test case
* test fixture class
* nonfatal failure
* fatal failure

### Assertions
Google Test assertions are macros that resemble function calls. You test a class or function by making assertions about its behavior. When an assertion fails, Google Test prints the assertion's source file and line number location, along with a failure message. You may also supply a custom failure message which will be appended to Google Test's message.

googletest 提供的 Assertion 有很多种，但常用主要是下面三种：

1. 直接判断测试结果为 True 或者 False，如 ASSERT_TRUE
2. 将测试结果与预定值比较，如 ASSERT_EQ
3. 比较两个 C string，如 ASSERT_STREQ

### Test Fixtures: Using the Same Data Configuration for Multiple Tests
Test Fixtures 用于在多个 Test 之间共享一份数据和代码。很常规的思想，但这里 googletest 把它抽象到了 Test Case 这一层，作为开发人员，我们只需要关心 Test Fixtures 的具体实现就可以了，而不用担心一个 Test Fixtures 对象的创建和释放。

下面是关于 Test Fixtures 的一些需要注意的点：

* Different tests in the same test case have different test fixture objects, and Google Test always deletes a test fixture before it creates the next one.

## More Assertions
一些不常用的 Assertion，主要分为如下三种：

1. 单纯的表示成功或者产生一个失败
2. 测试特定的代码块有没有抛出指定的异常
3. 能够定制测试成功或失败时的输出消息

### Predicate Assertions for Better Error Messages
下面是文档中所说的为什么要引入这种 Assertion 的说法：

> Sometimes a user has to use EXPECT_TRUE() to check a complex expression, for lack of a better macro. This has the problem of not showing you the values of the parts of the expression, making it hard to understand what went wrong.

但实际情况并不是这样。下面是使用 EXPECT\_PRED2 和 EXPECT\_GT 进行同样的判断在失败时两者分别输出的信息：

    // EXPECT\_GT 输出的信息
    ../samples/sample1_unittest.cc:90: Failure
    Expected: (Factorial(-10)) > (2), actual: 1 vs 2

    // EXPECT\_PRED2 输出的信息
    ../samples/sample1_unittest.cc:94: Failure
    MyGreater(Factorial(-10), 2) evaluates to false, where
    Factorial(-10) evaluates to 1
    2 evaluates to 2

如上所述，并没有出现文档中所说的情况，而且 EXPECT\_GT 输出的信息比 EXPECT\_PRED2 输出的信息更简洁。

另外，这小节涉及到的 Using a Function That Returns an AssertionResult 和 Using a Predicate-Formatter 这里就不说了，基本用不到。

## Floating-Point Comparison
一些专门用于浮点数比较的 Assertion，原因如下：

> Due to round-off errors, it is very unlikely that two floating-points will match exactly.

暂时不用关心这一块。

## Windows HRESULT assertions
如题，没什么可解释的，挺有用的。

## Type Assertions
就是下面这个函数：

    ::testing::StaticAssertTypeEq<T1, T2>();

当 T1 和 T2 的类型不一样时会导致编译错误。文档中的说法是这个函数主要用于模板类的成员函数中或者模板函数中。但暂时对这个函数的用途没什么实际的体会。

## Assertion Placement
googletest 中的 Assertion 可以像 C 标准库中的 assert 函数一样用在项目代码中。但是用不用真不是这几句话能说清楚的。

具体试了一下，EXPECT 系列的 Assertion 在出错时只是打印相关信息，不会中断代码的正常执行；而 ASSERT 系列的 Assertion 在出错时除了打印相关信息外，还会立刻从当前函数返回，然后代码继续运行；总之，和 C 标准库中的 assert 函数的行为还是不一样的。如果用的话，应该是用来打印一些 log，而是不像 C 标准库中的 assert 函数那样使用。

## Teaching Google Test How to Print Your Values
googletest 中的 Assertion 在打印相关信息时需要被打印的变量的类型支持 << 操作符，这里所谓的 Teaching Google Test How to Print Your Values 其实就是为一些不支持的 << 操作符的类型重载 << 操作符，这样就可以改善 googletest 中的 Assertion 在打印这些类型的变量时的信息的可读性。

## Death Tests
平常在项目中，我们会在代码中插入一些 assertions（如 C 标准库中的 assert 函数），这些 assertions 负责检查一些在程序运行时本不该出现的情况；当这些情况出现时，就让程序崩溃在出错的地方，这样开发人员就可以很快定位到哪里出错，并解决问题。

什么叫 Death Tests 呢？Death Tests 的目的就是负责检查代码中这些 assertions 的正确性。具体来讲，就是让程序运行在会导致这些 assertions 失败的情况下，然后检查这些 assertions 有没有正常工作，即看程序有没有按应有的方式崩溃，故叫 Death Tests。

另外在 Death Tests 中所说的 assertions 其实是一个广泛的概念，并不是局限于 c 标准库中的 assert 函数或类似函数，而是和 assert 函数功能类似的代码。

### How to Write a Death Test
写 Death Test，具体就是使用下面这些宏：

1. ASSERT\_DEATH(statement, regex) 或者 EXPECT\_DEATH(\_statement, regex\_)
2. ASSERT\_DEATH\_IF\_SUPPORTED(statement, regex) 或者 EXPECT\_DEATH\_IF\_SUPPORTED(\_statement, regex\_)
3. ASSERT\_EXIT(statement, predicate, regex) 或者 EXPECT\_EXIT(\_statement, predicate, regex\_)

关于这些宏的使用，只需要如下几个地方：

1. statement
    statement 指定要测试的 assertion 和让该 assertion 失败的条件；从代码的角度来讲，这里其实就是以指定参数调用某些函数，而这些函数就是要测试的 assertion 的代码；应该说，函数是 Death Test 能够测试的最低粒度。
2. regex
    regex is a regular expression that the stderr output of statement is expected to match
3. predicate
    predicate is a function or function object that evaluates an integer exit status

另外，上面提到 Death Tests 最后要检查程序有没有按预定的方式崩溃；具体讲，这一步的检查涉及到两个方面：进程的退出值和 stderr 的输出。进程的退出值通过 predicate 来检查；stderr 的输出通过 regex 来检查。

### How It Works
对文档中的描述不太理解，主要是这里涉及到了 POSIX systems 和 Windows 平台创建子进程的实现机制，但是暂时也没有必要对这一块进行深究。

下面是文档中这部分的的大致意思：

* 在 POSIX systems 平台上，当采用 fast death test style 时，在生成子进程后，只运行相应的 Death Test；在采用 threadsafe death style 时，在生成子进程后，会重新运行和这个子进程相关的所有单元测试。
* 在 Windows 系统上，在生成子进程后，会先让这个子进程正常执行（初始化什么的，注意这里和上面的区别），然后在运行指定的 Death Test。

### Death Tests And Threads
同上，不太理解，暂时不用深究。

下面是文档中这部分的的大致意思：

* 存在两种 death test styles（即上面提到的 fast 和 threadsafe）的原因是为了线程安全
* Due to well-known problems with forking in the presence of threads, death tests should be run in a single-threaded context
* 当 death test 无法在单线程环境中运行时，googletest 为减小线程安全所带来的问题而采取了一些措施

上面提到的 well-known problems，有时间可以查下。

### Death Test Styles
讨论了 threadsafe death test style，大致包含如下几点：

1. The "threadsafe" death test style was introduced in order to help mitigate the risks of testing in a possibly multithreaded environment.
2. The "threadsafe" death test style trades increased test execution time (potentially dramatically so) for improved thread safety
3. We suggest using the faster, default "fast" style unless your test has specific problems with it.

简单来讲，通常用 "fast" style 就够了，这也是 googletest 默认使用的 death test styel，有问题了在用 "threadsafe" style。

### Caveats
等出错了，在回头看。

## Using Assertions in Sub-routines
### Adding Traces to Assertions
下面是引入这个特性的原因：

 > If a test sub-routine is called from several places, when an assertion inside it fails, it can be hard to tell which invocation of the sub-routine the failure is from.

 这其实很像在开启了多个工作线程的项目中，需要跟踪每个工作线程的状态的需求；而且解决思路也是大同小异，都是打印一些能标识每次调用或者每个工作线程身份的信息。不过，googletest 把这种思想抽象成了一个函数：

     SCOPED_TRACE(message);

至于这个函数怎么用？看情况，可能只需要在函数入口处调用一下；也可能需要在每次调用这个函数的地方调用一下；总之，只要能达到目的，随你怎么用。

### Propagating Fatal Failures
ASSERT 系列的 Assertion 在出现错误时，默认情况下是从当前函数返回，然后代码会继续执行。但有些情况下，我们希望这个错误继续往下传，对之后的代码也产生影响。

下面是两种解决方案。

#### Asserting on Subroutines
不明白下面这段话的意思：

> Only failures in the thread that executes the assertion are checked to determine the result of this type of assertions. If statement creates new threads, failures in these threads are ignored.

#### Checking for Failures in the Current Test
就是 3 个函数，用于检测当前 TEST是否已经存在 Fatal Failure、NoFatal Failure 或者二者其一，我们可以通过其检测结果手动从当前 TEST 返回。

* HasFatalFailure()
* HasNonfatalFailure()
* HasFailure()

## Logging Additional Information
不明白有什么用

## Sharing Resources Between Tests in the Same Test Case
就是在 test fixture class 中定义一些静态成员，这样在同一个 Test Case 的不同 Test 中就可以共享一份资源了，而且最主要的是不用每次在开始一个新的 Test 时对这些资源进行初始化。

另外注意下下面两个函数的作用和调用时机。

    static void SetUpTestCase();
    static void TearDownTestCase() ;

## Global Set-Up and Tear-Down
单元测试程序层面的 Set-UP 和 Tear-Down 操作。

## Value Parameterized Tests
Value-parameterized tests allow you to test your code with different parameters without writing multiple copies of the same test.

简单来讲，就是每次用不同的值初始化同一个 Text Fixture，然后运行这个 Test Fixture 的所有 Test。

但具体怎么用就看情况了，比如在官方的示例中，用于初始化 Text Fixture 的值是一个父类的两个不同子类的实例，等于说用同样的一套 TEST 测试了一个父类的两个不同子类的实现；而[《玩转Google开源C++单元测试框架Google Test系列(gtest)之四 - 参数化》][1]这篇文章中则是因为在一个 TEST 中，一个 Assertion 要测试的函数因为传入的参数不同因而要写多个 一样的Assertion，于是使用了这个特性。两者比较，我觉得后者并不是一个很好的使用示例，有些滥用该特性，在这个粒度上使用该特性真的不太合适。

具体实践没什么说的，文档和实例代码说的很清楚了。

### Creating Value-Parameterized Abstract Tests
下面是我对这个特性的理解：

将 Value-Parameterized Test Fixture Class 的声明和实现分开，然后和这个类关联的所有 TEST 也单独写到一个文件中；这样我们可以根据情况使用这个 Value-Parameterized Test Fixture Class 的不同实现。

## Typed Tests
用同一个 Test Case 测试不同的类型；当然这些被测试的不同类型之间也是有关联的，比如是同一个父类的不同子类，总之要有能用同一个 Test Case 进行测试的共同点。

## Type-Parameterized Tests
Type-parameterized tests are like typed tests, except that they don't require you to know the list of types ahead of time. Instead, you can define the test logic first and instantiate it with different type lists later.

这个和上面这个的区别在于可以选择 Test Case 中的哪些 Test 运行，这个特性应该是用于某些 TEST 并不是适用于该 Test Case 要测试的所有类型这种情况。

## Testing Private Code
通常，我们不用测试所谓的 Private Code，即使存在这个需求，那也很大程度上是因为代码的组织不好，这时首先要考虑的是重构代码；但如果真的有这个需求，也不是不可以。

所谓的 Private Code 具体分为如下两种：

* Static functions (not the same as static member functions!) or unnamed namespaces, and
* Private or protected class members

而针对这两种 Private Code 的测试方式也不同。
### Static Functions
不太明白说了什么。但明显，googoletest 并没有为测试 Static Function 提供解决方案，这里所讨论的方法，其实都是基于 C++ 的语法进行变通的。

### Private Class Members
同上，googletest 也没有为这种情况提供什么解决方案。

最后，我觉得我们永远不会有这个需求，即使有，我也不会写。

## Catching Failures
这小节其实讲了这样一个问题，如何用 googletest 测试 googletest？在这种情况，其实是要用 googletest 的 assertion 测试 googletest 的 assertion，怎么做？因为 googletest 的 assertion 不抛出异常，所以之前提到的用于测试异常的 assertion 是用不了的，但是 googletest 专门提供了 assertion 用于处理这种情况。

## Getting the Current Test's Name
获取 Test 的名字等信息。怎么用就看需求了。不过，用到的可能性不大。

## Extending Google Test by Handling Test Events
下面这段话已经把怎么扩展和一些进行扩展的例子讲的很清楚了。

> Google Test provides an event listener API to let you receive notifications about the progress of a test program and test failures. The events you can listen to include the start and end of the test program, a test case, or a test method, among others. You may use this API to augment or replace the standard console output, replace the XML output, or provide a completely different form of output, such as a GUI or a database. You can also use test events as checkpoints to implement a resource leak checker, for example.

具体实现很简单，这里就不说了，但如要要在处理一些 event 时使用 googletest 中的 assertion，有一些点需要注意下，具体见  Generating Failures in Listeners 这一小节。

## Running Test Programs: Advanced Options
测试程序可以通过一些标志影响自身行为，这些标志可以通过环境变量或者命令行参数的形式传递给测试程序；另外在代码中，也可以直接读取或设置这些标志。

下面是对这些标志的介绍。

1. --gtest\_list\_tests
      List the names of all tests instead of running them. The name of
      TEST(Foo, Bar) is "Foo.Bar".

2. --gtest\_filter=POSTIVE\_PATTERNS[-NEGATIVE\_PATTERNS]
    Run only the tests whose name matches one of the positive patterns but none of the negative patterns. '?' matches any single character; '*' matches any substring; ':' separates two patterns.

3. --gtest\_also\_run\_disabled\_tests
    Run all disabled tests too.

4. --gtest_repeat
    Run the tests repeatedly; use a negative count to repeat forever.
    这个功能主要用于有些错误只是偶尔会出现的情况下。
5. --gtest_shuffle
    Randomize tests' orders on every iteration. Randomize tests' orders on every iteration.
    然而感觉并不会用到。
6. --gtest\_output=xml[:DIRECTORY\_PATH/|:FILE\_PATH]
    Generate an XML report in the given directory or with the given file name. FILE\_PATH defaults to test\_details.xml.
    这个功能未来也许会用到，主要是考虑到如下这点：

    > --gtest_output=xml[:DIRECTORY_PATH/|:FILE_PATH]
      Generate an XML report in the given directory or with the given file
      name. FILE_PATH defaults to test_details.xml.
7. --gtest\_break\_on\_failure
    Turn assertion failures into debugger break-points.

      When running test programs under a debugger, it's very convenient if the debugger can catch an assertion failure and automatically drop into interactive mode. Google Test's break-on-failure mode supports this behavior.

      说是这么说，感觉还是不会用到。
8. --gtest\_catch\_exceptions=0
      Do not report exceptions as test failures. Instead, allow them
      to crash the program or throw a pop-up (on Windows).

      跟 --gtest\_break\_on\_failure 差不多，感觉用不到
9. --gtest_throw_on_failure
    Turn assertion failures into C++ exceptions.
      文档中给的这个标志的使用情景是在基于另外一种单元测试框架写的测试程序中使用 googletest 的 assertion。但我觉得这样做本身就是一个很愚蠢的行为。

### Temporarily Disabling Tests
可以通过给 Test Name 或者 Test Case Name 前面添加 DISABLED\_ 使特定 Test 或者特定 Test Case 的所有 Test 失效，即在运行时不执行这些 Test。

## Distributing Test Functions to Multiple Machines
看意思是说 googletest 天生支持分布式。很好，但具体怎么用，等有这个需求再说吧。

[1]:    http://www.cnblogs.com/coderzh/archive/2009/04/08/1431297.html    "玩转Google开源C++单元测试框架Google Test系列(gtest)之四 - 参数化"
[2]:    https://github.com/google/googletest/blob/master/googletest/docs/Primer.md    "Primer"
[3]:    https://github.com/google/googletest/blob/master/googletest/docs/AdvancedGuide.md    "AdvancedGuide"
[4]:    http://blog.csdn.net/u013482618/article/details/50107943    "漫谈单元测试"
