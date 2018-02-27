# cvtColor()
Converts an image from one color space to another.

In case of linear transformations, the range does not matter. But in case of a non-linear transformation, an input RGB image should be normalized to the proper value range to get the correct results, for example, for RGB → L*u*v* transformation. For example, if you have a 32-bit floating-point image directly converted from an 8-bit image without any scaling, then it will have the 0..255 value range instead of 0..1 assumed by the function. So, before calling cvtColor , you need first to scale the image down:

    img *= 1./255;
    cvtColor(img, img, COLOR_BGR2Luv);

上面这段话涉及到了 linear transformations 和 non-linear transformation，这里先不深究这两者的定义，需要注意的是，在 non-linear transformation 中，an input RGB image should be normalized to the proper value range to get the correct results。

If you use cvtColor with 8-bit images, the conversion will have some information lost. For many applications, this will not be noticeable but it is recommended to use 32-bit images in applications that need the full range of colors or that convert an image before an operation and then convert back.

上面提到的对 8-bit images 使用 cvtColor 导致图片的信息丢失，所以在那些不允许这种情况发生的应用中，应该使用 32-bit images，来避免这种问题。


[1]: https://docs.opencv.org/3.3.1/d7/d1b/group__imgproc__misc.html#ga397ae87e1288a81d2363b61574eb8cab "cvtColor"