以 windows 10 x64，VS2015、boost 1_60_0 为例，编译 boost 32 位库，通常只需要打开 VS2015 x86 本机工具命令提示符，然后进入 boost 源代码目录下执行如下命令就可以了：

    bootstrap
    .\b2

但仅仅是这样并不够。因为这种情况下，会编译所有模块，会编译每个模块的所有版本，然后编译的是 32 位库；但如果我们只想编译指定模块，只想编译模块的特定版本，想编译 64位库，想指定编译后的头文件和库文件的 install 目录怎么办？这种情况下，我们就需要在执行 b2 时指定一些选项。

# 编译模块的特定版本
boost 库在编译时有很多选项，将这些选项进行组合，可以编译出 boost 库的不同版本。下面是这些选项：

1. threading；单线程、多线程
2. threading；static（静态库）、share（动态库）
3. variant；debug、release
4. runtime-link；Whether to link to static or shared C and C++ runtime. 

具体编译时，通常我们只需要编译出需要的版本即可，并不需要将所有组合都进行编译。通常编译的组合如下：

1. 多线程  runtime-link=share 静态库 debug
2. 多线程  runtime-link=share 静态库 release
3. 多线程  runtime-link=share 动态库 debug
4. 多线程  runtime-link=share 动态库 release

上面这 4 中组合就是常用的。

# 编译 64 位库
编译 64 位库和编译 32 位库步骤基本一致，区别如下

1. 打开 VS2015 x64本机工具命令提示符
2. 调用 b2 时指定如下选项

        address-model=64

# 只编译指定的模块
调用 b2 时使用如下选项

1. --with-<library>
2. --without-<library>

# 指定编译后的库的头文件和库文件的 install 目录
--prefix=<PREFIX>       Install architecture independent files here.
                        Default; C:\Boost on Win32
                        Default; /usr/local on Unix. Linux, etc.

--exec-prefix=<EPREFIX> Install architecture dependent files here.
                        Default; <PREFIX>

--libdir=<DIR>          Install library files here.
                        Default; <EPREFIX>/lib

--includedir=<HDRDIR>   Install header files here.
                        Default; <PREFIX>/include


## 示例
### 示例1
.\b2 install  --prefix="D:\ThirtyLib\boost" --libdir="D:\ThirtyLib\boost\x64\vc14\lib" --with-filesystem address-model=64 threading=multi link=static,shared

上述命令会编译单独编译 64位的 filesystem 模块的 4 个版本（上面提到的），并将头文件放到 D:\ThirtyLib\boost\include 目录下，库文件放到 D:\ThirtyLib\boost\x64\vc14\lib 目录下。

### 示例2
.\b2 install  --prefix="D:\ThirtyLib\boost" --libdir="D:\ThirtyLib\boost\x64\vc14\lib" --with-filesystem threading=multi link=static,shared

上面的命令会单独编译 32 位的 filesystem 模块的 4 个版本。

## 示例3
.\b2 install  --prefix="D:\ThirtyLib\boost" --libdir="D:\ThirtyLib\boost\x64\vc14\lib"  threading=multi link=static,shared

上述命令会编译32位的所有模块的4个版本。

## 示例4
.\b2 install --prefix="D:\ThirtyLib" --exec-prefix="D:\ThirtyLib\bin" --includedir="D:\ThirtyLib\include" --libdir="D:\ThirtyLib\lib\x64\vc14" --with-program_options  address-model=64 threading=multi link=static,shared

# Auto-Linking
Most Windows compilers and linkers have so-called “auto-linking support,” which eliminates the second challenge. Special code in Boost header files detects your compiler options and uses that information to encode the name of the correct library into your object files; the linker selects the library with that name from the directories you've told it to search.

VS2015 是支持 Auto-Linking，但有一个问题，以 filesystem 模块为例，auto link 的库是静态库，如果我们想 link 动态库怎么办啊？在预处理器中添加 BOOST_DYN_LINK 就可以了。

具体可以参考 boost\config\auto_link.hpp 文件中关于 BOOST_DYN_LINK 的注释和相关代码。

    BOOST_DYN_LINK:           Optional: when set link to dll rather than static library.

    //
    // select linkage opt:
    //
    #if (defined(_DLL) || defined(_RTLDLL)) && defined(BOOST_DYN_LINK)
    #  define BOOST_LIB_PREFIX
    #elif defined(BOOST_DYN_LINK)
    #  error "Mixing a dll boost library with a static runtime is a really bad idea..."
    #else
    #  define BOOST_LIB_PREFIX "lib"
    #endif
    
[1]: http://www.boost.org/doc/libs/1_65_1/more/getting_started/windows.html "Getting Started on Windows"
[2]: http://www.boost.org/build/doc/html/bbv2/overview/invocation.html "Boost.Build"
[3]: http://www.360doc.com/content/15/0306/16/1887516_453110300.shtml "动态链接 boost 库"