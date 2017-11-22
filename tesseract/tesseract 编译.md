# 介绍
This package contains an OCR engine - libtesseract and a command line program - tesseract.

# 环境
Windows 10 x64; VS2015；tesseract 4.0 ebbfc3a

# 需求
基于 libtesseract 开发程序。

# 编译 32 位版本 libtesseract 流程
# 第一步
下载 [CPPAN] [1]，然后将 cppan 的执行路径加入到 PATH 环境变量中

## 第二步
获取 tesseract 最新的源码

    git clone https://github.com/tesseract-ocr/tesseract tesseract

## 第三步

    cd tesseract
    cppan
    mkdir build && cd build
    cmake ..

上述命令会在 tesseract\build 目录下生成一个名为 tesseract.sln 的解决方案，直接打开该解决方案，直接编译 libtesseract 项目和 tesseract 项目即可。

通过上面命令生成的 libtesseract 项目，会生成 dll 版本。

在编译 libtesseract 项目的过程中，会出现一些编译错误。

然后，编译成功后，生成的 dll 文件在 tesseract\build\bin\debug 或者 tesseract\build\bin\release 目录下，对应的 lib 文件在 tesseract\build\debug 和 tesseract\build\release 目录下。

# 编译 64 位版本 libtesseract 流程
第一步和第二步和编译 编译 32 位版本 libtesseract 一样

## 第三步

    mkdir win64 && cd win64
    cppan ..
    cmake .. -G "Visual Studio 14 2015 Win64"

执行完上述命令后，会在 tesseract\win64 目录下生成名为 tesseract.sln 的解决方案，编译 libtesseract 项目即可。

编译过程中出现的错误和编译 32 位 tesseract 一样，这些就不在叙述。

# 如何使用 libtesseract
## 问题一：提取头文件
使用 libtesseract 要解决的第一个问题就是提取头文件，libtesseract 并没有将基于它进行开发时需要的各种头文件集中起来放在一起。所以，需要我们手动提取。那如何确定需要提取哪些头文件呢？提取出来后，怎么组织这些头文件呢？

第一个问题，如何如何确定需要提取那些头文件？答案是基于 tesseract 项目确定需要提取的头文件，因为 tesseract 项目是基于 libtesseract 开发的。

第二个问题，怎么组织这些头文件？直接将这些头文件放到一个目录下即可。

## 问题二：提取相关的 lib 和 dll 文件
基于 libtesseract 库进行开发，单独提取 libtesseract 库的 dll 和 lib 是不够的。

还需要提取 leptonica 库的 dll 和 lib 文件。因为 libtesseract 基于  leptonica 库开发，而且在基于 libtesseract 进行开发时，libtesseract 也需要配合 leptonica 库使用。

那如何确定 leptonica 库 dll 和 lib 文件的位置呢？首先在 libtesseract 项目属性中的附加依赖性属性中，我们可以找到 leptonica 库的 lib 文件的位置；然后运行 copy-dependencies 项目，该项目会将 libtesseract 需要的所有 dll 文件拷贝到 tesseract\build\bin\debug 或者 tesseract\build\bin\release 目录下，在其中就可以找到 leptonica 库的 dll 文件。

但仅仅这样还不够，因为 leptonica 又依赖于一些 dll，我们需要把这些 dll 也提取出来，下面是这些 dll 的列表：

1. pvt.cppan.demo.xz_utils.lzma-5.2.3.dll
2. pvt.cppan.demo.webp-0.6.0.dll
3. pvt.cppan.demo.tiff-4.0.8.dll
4. pvt.cppan.demo.png-1.6.33.dll
5. pvt.cppan.demo.openjpeg.openjp2-2.3.0.dll
6. pvt.cppan.demo.madler.zlib-1.2.11.dll
7. pvt.cppan.demo.jpeg-9.2.0.dll

在运行 copy-dependencies 项目时，这些文件也会被拷贝到 tesseract\build\bin\debug 或者 tesseract\build\bin\release 目录下。

总结来讲，在使用 libtesseract 时，我们需要提取下面这些文件：

* libtesseract 的头文件、lib 文件、dll 文件
* leptonica 的 lib 文件、dll 文件
* pvt.cppan.demo.xz_utils.lzma-5.2.3.dll
* pvt.cppan.demo.webp-0.6.0.dll
* pvt.cppan.demo.tiff-4.0.8.dll
* pvt.cppan.demo.png-1.6.33.dll
* pvt.cppan.demo.openjpeg.openjp2-2.3.0.dll
* pvt.cppan.demo.madler.zlib-1.2.11.dll
* pvt.cppan.demo.jpeg-9.2.0.dll

有了这些文件，才能基于 libtesseract 进行开发。

[1]: https://cppan.org/client/ "CPPAN"
[2]: http://www.jianshu.com/p/0a3386227981 "Tesseract-OCR学习系列（一）简介"
[3]: https://github.com/tesseract-ocr/tesseract/wiki/Compiling#windows "Compilation guide for various platforms"