编辑 openssl 32 位版本

# 环境
win10 x64、Visual Studio 2008、openssl-1.0.1t

# 步骤

第一步，下载安装 ActiveState Perl 和 NASM，将两者都加入到系统路径中

    http://www.activestate.com/ActivePerl

    http://nasm.sourceforge.net/

第二步，打开 Visual Studio 2008 命令提示，进入 openssl-1.0.1t 源码根目录， 在 openssl-1.0.1t 源码根目录下按顺序执行如下命令

    perl Configure VC-WIN32 --prefix=c:/openssl
    ms\do_nasm
    nmake -f ms\ntdll.mak
    nmake -f ms\ntdll.mak install

    编译后的所有文件位于 c:/openssl 目录下


上述步骤参考 openssl-1.0.1t 源码根目录下的 INSTALL.32 文档完成。