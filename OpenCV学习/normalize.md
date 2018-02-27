# normalize 函数
Normalizes the norm or value range of an array.

归一化输入数组使它的范数或者数值范围在一定的范围内。


重点还是理解归一化的定义。但目前理解并不是很深，就我们关注的将数组的数字范围限制在一定范围内这一点来讲，结合下面的公式

    if mask(i,j)!=0
        dst(i,j) = (src(i,j) - min(src)) * (b' - a') / (max(src) - min(src)) +  a'
    else
         dst(i,j) = src(i,j)

    其中b' = MAX(alpha,beta ) , a' = MIN(alpha,beta );

多少能有有一点理解。

范数这个暂时就不了解。

# 参考文档
1. [归一化函数normalize详解][1]

[1]: http://blog.csdn.net/solomon1558/article/details/44689611 "归一化函数normalize详解"

