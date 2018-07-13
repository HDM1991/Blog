# 环境
win10 x64、vs2008、boost 1.60、cmake 3.11.4、cpp-netlib-0.11.2

注意这里编译的 cpp-netlib 的版本是 0.11.2，这是因为 cpp-netlib 自 0.11.2 之后的版本都使用了 C++ 11 特性，而 VS 2008 并不支持 C++ 11 特性，所以使用 VS 2008 编译高于 0.11.2 的版本会出错。

# 步骤
1. 编译 boost 1.60
    
    .\b2 install threading=multi link=static,shared
    
    注意上面的命令没有指定编译文件输出目录，所以使用默认目录 C:\Boost
2. 编译 openssl

3. 打开 Visual Studio 2008 命令提示，进入 cpp-netlib 源码根目录执行如下命令：
    
        mkdir build
        cd build
        cmake ../
    
    注意在上面的步骤中，没有指定 boost 和 openssl，这是因为如果在之前编译 boost 和 openssl 时，将他们都放在 C 盘根目录下，上述命令会自动检测到它们的存在。当然，也可以通过 -DOPENSSL_ROOT_DIR 和 -DBOOST_ROOT 指定 boost 和 openssl 头文件的目录。

4. 使用 cmake 生成 VS 2008 的解决方案文件
    
5. 用 VS2008 打开生成的解决方案文件，进行编译
    在进行编译时发现，cppnetlib-server-parsers 项目会编译失败，但我们不使用这个模块，所以暂时就不解决这个问题了。

[1]: https://github.com/cpp-netlib/cpp-netlib "cpp-netlib"
