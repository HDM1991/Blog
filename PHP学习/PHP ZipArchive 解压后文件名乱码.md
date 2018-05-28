# 原因
当压缩包的中的文件名非 utf-8 编码时，ZipArchive 依然会以 utf-8 编码的方式解释文件名

# 解决思路
修改压缩包，将所有文件名改成 utf-8 编码后在解压，示例代码如下所示：

    $zip = new ZipArchive;
    if ($zip->open($file) === TRUE) {
        $count = $zip->numFiles;
        for ($i=0; $i < $count; $i++) { 
            $stat = $zip->statIndex($i, ZipArchive::FL_ENC_RAW);
            
            $n = iconv("GBK", "UTF-8//IGNORE",  $stat['name']);
            $zip->renameIndex($i, $n);
        }

       $zip->close();
    } else {
       exit(__LINE__ . "\t打开压缩文件失败。文件名: {$file}\n");
    }


需要注意的是 ZipArchive::FL_ENC_RAW, 它的作用是 Get unmodified string.

# 参考文档

* [《PHP ZipArchive类 文件名中文乱码如何解决》][1]

[1]: https://blog.csdn.net/qq_40349986/article/details/79225560 "PHP ZipArchive类 文件名中文乱码如何解决"