# 概述
记得从看《Windows程序设计》时就遇到过这个问题，字符编码，字符集傻傻分不清楚，每次感觉弄清楚了，但不久遇到后又混乱了。所以这次记下一些自己的理解。

# 什么是字符集
简而言之，即使一些字符的集合。不同的字符集包含不同的字符。

# 什么是字符集编码
如上所说，字符集只是一些字符的集合，并没有定义这些字符如何在计算机内存中如何表示。于是，字符集编码就出现了，字符集编码对字符集中的每一个字符进行编码，这个编码一方面会给每一个字符一个唯一的标识或者说编号，另一方面也规定了这个唯一的标识在计算机内存中的表现形式。一个字符集可能有多个字符集编码。比如现在针对中文，我们制定了一个字符集叫 Chinese，然后该字符集对应的字符集编码为 Chinese-c，然后 Chinese-c 规定 'f' 和 '你' 这两个字符的标识分别为 0x1和 0xabcdffff，然后标识在内存中均占两个字节，遵循大字节序。当然，你也可以另针对该字符集制定另外一个字符集编码。给每一个字符不同的唯一标识或者规定标识在内存中的不同表现形式。

# 为什么会出现这么多的字符集和字符集编码呢？
问题在于，世界上很多国家，很多语言，不同的语言通常也意味着不同的字符。拿我大天朝举例，我们使用汉字，现在回到当初制定字符集的时候，大家身临其境的考虑下，首先可以肯定的是，这个字符集肯定是要包含所有的汉字字符的。嗯，那是否要考虑其他语言的字符呢？答案是，你当然可以考虑，但这不是一个一时半会能解决的问题，那么多语言，那么多字符，这是一个相当庞大的工程，还有就是，假如我们完成了这个工程，为世界上的所有字符制定了一个统一的字符集，那其他国家会使用这个字符集吗，不使用这个字符集，我们不是白干了。于是，我们制定了一个只包含中文字符的字符集 Chinese 和针对该字符集的字符集编码 Chinese-c。然后，其他国家也是这么想的，于是就出现了这么多的字符集和字符集编码。

