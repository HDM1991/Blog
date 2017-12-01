# 简介
C++ 第三方库的管理有多头疼，想必大家都有体会。但做一个跨平台的 C++ 库管理工具有多难，只能说至今没见到一个能用的 C++ 第三方库管理工具。

但假如不做一个跨平台的 C++ 库管理工具，只做一个 Windows 平台下的，只需要支持特定版本的 Visual Studio 的库管理工具呢？

Microsoft 就真的做了这样一个工具 - [vcpkg][1]。

vcpkg 只支持 windows 平台，而且只支持  Visual Studio 2015 Update 3、Visual Studio 2017。

使用 vcpkg +  VS2015 或 VS2017，可以做到:

1. 不用在担心用到的库如何编译
2. 不用在配置 include、lib、bin 目录
3. 不用在项目链接属性下指定要链接那些库

我们需要做的就是通过 vcpkg 安装库之后，在代码中 include 相应的头文件，然后直接写代码就可以了。

# 使用
在正式使用 vcpkg 安装各种库之前，需要先执行做如下操作：

1. 执行如下命令克隆 vcpkg 的项目仓库到本地

        git clone https://github.com/Microsoft/vcpkg.git

2. 在本地 vcpkg 项目仓库的根目录下执行如下命令，生成 vcpkg

        C:\src\vcpkg> .\bootstrap-vcpkg.bat

3. 在本地 vcpkg 项目仓库的根目录下执行如下命令，hook up user-wide integration, run (note: requires admin on first use)

        C:\src\vcpkg> .\vcpkg integrate install

    只有执行上述命令后，才可以在使用 VS 开发项目时，不在做配置目录之类的工作。

下面介绍下 2 个常用的命令。
## search

    PS D:\src\vcpkg> .\vcpkg search sqlite
    libodb-sqlite        2.4.0            Sqlite support for the ODB ORM library
    sqlite3              3.15.0           SQLite is a software library that implements a se...

    If your library is not listed, please open an issue at:
        https://github.com/Microsoft/vcpkg/issues

## install
### 示例一：安装 32 位 sqlite3

    PS D:\src\vcpkg> .\vcpkg install sqlite3

### 示例二：安装 64 位 sqlite3

    PS D:\src\vcpkg> .\vcpkg install sqlite3:x64-windows

另外，目前 vcpkg 支持如下几种创建类型：

    PS E:\Documents\Github\vcpkg> .\vcpkg help triplet
    Available architecture triplets:
      arm-uwp
      arm64-uwp
      x64-uwp
      x64-windows-static
      x64-windows
      x86-uwp
      x86-windows-static
      x86-windows

执行 install 命令时不指定创建类型（如示例一），默认就是创建 32 版本 dll 库（包括 deubg 和 release 版）。

指定 x64-windows 创建 64 位版本 dll 库（deubg 和 release 版）。

指定 x64-windows-static 创建 64 位版本静态库（deubg 和 release 版）。

# 问题
This tool and ecosystem are currently in a preview state。

vcpkg 这个项目目前还处于开发中的阶段，尝试安装了一些库之后，还是存在有些库不能成功编译的情况。

[1]: https://github.com/Microsoft/vcpkg "Microsoft/vcpkg"