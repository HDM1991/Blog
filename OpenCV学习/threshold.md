# threshold
很容易理解，需要注意如下点：

Also, the special values cv::THRESH_OTSU or cv::THRESH_TRIANGLE may be combined with one of the above values. In these cases, the function determines the optimal threshold value using the Otsu's or Triangle algorithm and uses it instead of the specified thresh . The function returns the computed threshold value. Currently, the Otsu's and Triangle methods are implemented only for 8-bit images.

上面这段话的意思就是，cv::THRESH_OTSU or cv::THRESH_TRIANGLE 能通过算法帮我们需要合适的阈值，然后 cv::THRESH_OTSU or cv::THRESH_TRIANGLE 和 THRESH_BINARY 、THRESH_BINARY_INV 等同时指定，比如：

    cv::THRESH_OTSU | THRESH_BINARY_INV

还有就是 the Otsu's and Triangle methods are implemented only for 8-bit images.

有一个需要担心的问题是，是在《【OpenCV3】阈值化操作——cv::threshold()与cv::adaptiveThreshold()详解》中提到的，直接阈值化操作是一种一刀切的方式，对于亮度分布差异较大的图像，常常无法找到一个合适的阈值，针对这种情况，需要一种改进的阈值化算法，即自适应阈值化。

这个点，后面可能需要注意下。

# 参考文献
1. [【OpenCV3】阈值化操作——cv::threshold()与cv::adaptiveThreshold()详解][1]

[1]: 直接阈值化操作是一种一刀切的方式，对于亮度分布差异较大的图像，常常无法找到一个合适的阈值。 "【OpenCV3】阈值化操作——cv::threshold()与cv::adaptiveThreshold()详解"
[2]: https://docs.opencv.org/3.3.1/d7/d1b/group__imgproc__misc.html#gae8a4a146d1ca78c626a53577199e9c57 "threshold"