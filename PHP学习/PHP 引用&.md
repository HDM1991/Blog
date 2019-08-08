References in PHP are a means to access the same variable content by different names. 

Instead, they are symbol table aliases.

Note that in PHP, variable name and variable content are different, so the same content can have different names. The closest analogy is with Unix filenames and files - variable names are directory entries, while variable content is the file itself. References can be likened to hardlinking in Unix filesystem.


[1]: http://blog.csdn.net/wenzhou1219/article/details/16832067 "1.深入PHP变量存储结构"

[2]: http://blog.csdn.net/wenzhou1219/article/details/16850737 "2.深入PHP赋值行为"

[3]: http://blog.csdn.net/wenzhou1219/article/details/16829905 "3.深入PHP中的引用"
[4]: http://php.net/manual/en/language.references.whatare.php "What References Are"