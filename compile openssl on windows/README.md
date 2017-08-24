这里使用的 openssl  版本为 openssl-1.1.0f

# 编译环境
## Perl
下载地址：https://www.perl.org/get.html#win32

ActiveState Perl 或者 Strawberry Perl 都可以
## nasm
下载地址：http://www.nasm.us

安装完成后，需要把 nasm 的根目录添加到 Path 环境变量中

# 编译步骤
这里以 Visual Studio 2015 为例

## 编译64位版本
Step 1：以管理员权限打开”VS2015 x64 本机工具命令提示符“。这里如果不使用”VS2015 x64 本机工具命令提示符“，可能会出现 fatal error LNK1112 错误。
Step 2：进入 openssl 源码根目录
Step 3：运行 perl Configure VC-WIN64A 或者 perl Configure VC-WIN64I 命令(具体使用哪个命令由主机CPU架构决定)
Step 4：运行 nmake 命令
Step 5：运行 nmake test 命令
Step 6：运行 nmake install 命令

执行完上述命令后，openssl 会被安装到 C:\Program Files\OpenSSL 目录下

## 编译32位版本
Step 1：以管理员权限打开”VS2015 x86 本机工具命令提示符“
Step 2：进入 openssl 源码根目录
Step 3：运行 perl Configure VC-WIN32 命令
Step 4：运行 nmake 命令
Step 5：运行 nmake test 命令
Step 6：运行 nmake install 命令

执行完上述命令后，openssl 会被安装到 C:\Program Files (x86)\OpenSSL 目录下

