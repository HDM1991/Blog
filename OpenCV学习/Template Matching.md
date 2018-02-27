# 前言
最近在一个项目中，需要做一些图像方面的处理。所以对用到的一些知识点总结下。


# What
Template matching is a technique for finding areas of an image that match (are similar) to a template image (patch).

Template matching 指基于一张 template image (patch) 在目标图片上找到和 template image (patch) 相似的区域。

# How
首先做如下约定：

1. Source image (I): The image in which we expect to find a match to the template image
2. Template image (T): The patch image which will be compared to the template image

怎么在 I 上寻找 T 呢？

抽象点讲就是，以 I 的左上角为原点（0，0），X 轴向右，Y 轴向下，以像素为单位，对 I 的每一个坐标，基于选中的匹配方法，计算一个 值 matrix (R) ，然后基于这个值来评判，这个位置所代表的那个区域与 template image 的相似度。

以 CV_TM_SQDIFF 为例，假如当前位置为（1，2）， T 的 w 为 10， h 为 10，则基于下列公式对 I 中（1，2）到（11，12）这个区域的像素值和 T 中对应位置的像素值进行计算，算出一个  matrix (R)，并将 R 存储到结果矩阵中的 （1，2）这个位置。

其实抛开上面这些废话，所谓的 Template matching 就是基于公式，计算目标区域和 template image 中每一个对应的像素值，最后得出一个 R，来代表这个区域和 template image 的相似度。核心就是这个公式。除开这个公式，并没有什么其他复杂的。我们应该注意的其实是每个公式的效果、性能，根据实际需求选择合适的公式。

# 参考文档
1. [Template Matching][1]

[1]: https://docs.opencv.org/2.4/doc/tutorials/imgproc/histograms/template_matching/template_matching.html "Template Matching"