source image f()
remapped image g()

g(x, y) = f(h(x, y))

即将 remapped image 每一个坐标 (x, y) 上的像素值， 映射到 source image 上的位置 h(x, y) 的像素值。

这里 h() 由我们在 remap 函数中通过参数 map1 和 map2 指定。

需要注意的一点是，对于 remapped image 的坐标 (x, y)，通过 h() 获取的 source image 上的坐标可能不是一个整数值，比如在[《Remapping》][1]中给出的例子中的第一种映射方式(将 source image 缩小了一般)就是这样，对于坐标 (26, 26)，通过 h() 获取的坐标为 (2.5, 2.5)，在这种情况下，我们需要通过 interpolation 参数指定合适的插值方式。

所以，这里最重要的一点，我觉得是插值这个概念。


关于插值算法，在图像处理中的应用，[《双线性插值算法进行图像缩放及性能效果优化》][5]这篇博客说的比较好，我得理解是主要在对图像进行放大或者缩小时，进行应用，比如在上面提到的例子，就属于缩小的情况。

然后，不同的插值算法，效果也不一样，这点可以参考[《图像插值技术综述》][4]。


就我们的应用而言，觉得目前用不到什么特别的映射，比如缩放，可以使用 cv:resize 函数，没必要用 remap 函数手动指定映射关系；然后后面可能会用到的旋转操作，应该也有现成的函数，毕竟这些都是最常用的映射关系。

[1]: https://docs.opencv.org/2.4/doc/tutorials/imgproc/imgtrans/remap/remap.html "Remapping"
[2]: http://www.cnblogs.com/linkr/p/3630902.html   "双线性插值"
[3]: https://wapbaike.baidu.com/item/%E6%8F%92%E5%80%BC/13014727    "插值"
[4]: https://wenku.baidu.com/view/3d2bb41252d380eb62946d47.html "图像插值技术综述"
[5]: http://www.cnblogs.com/funny-world/p/3162003.html "双线性插值算法进行图像缩放及性能效果优化"
[6]: https://docs.opencv.org/3.3.1/da/d54/group__imgproc__transform.html#gab75ef31ce5cdfb5c44b6da5f3b908ea4 "remap()
"

