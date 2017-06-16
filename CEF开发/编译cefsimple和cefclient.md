# 下载
http://opensource.spotify.com/cefbuilds/index.html

# BUILD REQUIREMENTS
The below requirements must be met to build this CEF binary distribution.
- CMake version 2.8.12.1 or newer.
- Windows requirements:
  Visual Studio 2010 or newer building on Windows 7 or newer. Visual Studio
  2015 Update 2 and Windows 10 64-bit are recommended.

# BUILD EXAMPLES
The below commands will generate project files and create a Debug build of all
CEF targets using CMake and the platform toolchain.

Start by creating and entering the CMake build output directory:

    \> cd path/to/cef\_binary\_*
    \> mkdir build && cd build

To perform a Windows build using a 32-bit CEF binary distribution:
  Using the Visual Studio 2015 IDE:
    > cmake -G "Visual Studio 14" ..
    Open build\cef.sln in Visual Studio and select Build > Build Solution.

  Using Ninja with Visual Studio 2015 command-line tools:
    (this path may be different depending on your Visual Studio installation)
    > "C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin\vcvars32.bat"
    > cmake -G "Ninja" -DCMAKE_BUILD_TYPE=Debug ..
    > ninja cefclient cefsimple
To perform a Windows build using a 64-bit CEF binary distribution:
  Using the Visual Studio 2015 IDE:
    > cmake -G "Visual Studio 14 Win64" ..
    Open build\cef.sln in Visual Studio and select Build > Build Solution.

  Using Ninja with Visual Studio 2015 command-line tools:
    (this path may be different depending on your Visual Studio installation)
    > "C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin\amd64\vcvars64.bat"
    > cmake -G "Ninja" -DCMAKE_BUILD_TYPE=Debug ..
    > ninja cefclient cefsimple

# 编译过程中存在的问题
在编译32位的 cefclient 和 cefsimple 的 Debug 版本时，会出现如下 bug：

    1>cef_sandbox.lib(sandbox_win.obj) : error LNK2038: 检测到“_ITERATOR_DEBUG_LEVEL”的不匹配项: 值“0”不匹配值“2”(binding_test.obj 中)
    1>cef_sandbox.lib(sandbox_win.obj) : error LNK2038: 检测到“RuntimeLibrary”的不匹配项: 值“MT_StaticRelease”不匹配值“MTd_StaticDebug”(binding_test.obj 中)

根据代码中的注释，是因为 cef_sandbox.lib 自身的问题存在的，所以在编译 Debug 版本时，去掉预处理器中的 CEF\_USE\_SANDBOX 就行了。

    #if defined(CEF_USE_SANDBOX)
    // The cef_sandbox.lib static library is currently built with VS2013. It may not
    // link successfully with other VS versions.

编译 Release 版本不会出现上述问题。