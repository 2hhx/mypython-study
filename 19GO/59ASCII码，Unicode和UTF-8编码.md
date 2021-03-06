## 一 ASCII码

计算机内部，所有信息最终都是一个二进制值。每一个二进制位（bit）有0和1两种状态，因此八个二进制位就可以组合出256种状态，这被称为一个字节（byte）。也就是说，一个字节一共可以用来表示256种不同的状态，每一个状态对应一个符号，就是256个符号，从00000000到11111111。  
上个世纪60年代，美国制定了一套字符编码，对英语字符与二进制位之间的关系，做了统一规定。这被称为 ASCII 码，一直沿用至今。

ASCII
码一共规定了128个字符的编码，比如空格SPACE是32（二进制00100000），大写的字母A是65（二进制01000001）。这128个符号（包括32个不能打印出来的控制符号），只占用了一个字节的后面7位，最前面的一位统一规定为0。

0 | 1 | 1 | 0 | 0 | 0 | 1 | 0  
---|---|---|---|---|---|---|---  
统一规定为0 | 0或1 | 0或1 | 0或1 | 0或1 | 0或1 | 0或1 | 0或1  
  
## 二 非ASCII编码

如果只表示英语，用128个符号编码就够了，但是用来表示其他语言，128个符号是不够的。

比如，在法语中，字母上方有注音符号，它就无法用 ASCII 码表示。

于是，一些欧洲国家就决定，利用字节中闲置的最高位编入新的符号。

比如，法语中的é的编码为130（二进制10000010）。

这样一来，这些欧洲国家使用的编码体系，可以表示最多256个符号。

**但是** ， **这里又出现了新的问题** 。

不同的国家有不同的字母，因此，哪怕它们都使用256个符号的编码方式，代表的字母却不一样。

比如，130在法语编码中代表了é，在希伯来语编码中却代表了字母Gimel (ג)，在俄语编码中又会代表另一个符号。

但是不管怎样，所有这些编码方式中，0--127表示的符号是一样的，不一样的只是128--255的这一段。

至于亚洲国家的文字，使用的符号就更多了，汉字就多达10万左右。

一个字节只能表示256种符号，肯定是不够的，就必须使用多个字节表达一个符号。

比如，简体中文常见的编码方式是 GB2312，使用两个字节表示一个汉字，所以理论上最多可以表示 256 x 256 = 65536 个符号

## 三 Unicode

世界上存在着多种编码方式，同一个二进制数字可以被解释成不同的符号。

因此，要想打开一个文本文件，就必须知道它的编码方式，否则用错误的编码方式解读，就会出现乱码。

这就是为什么文件经常出现乱码，因为编码和解码用的编码方式不一样

可以想象，如果有一种编码，将世界上所有的符号都纳入其中。每一个符号都给予一个独一无二的编码，那么乱码问题就会消失。

这就是 Unicode，就像它的名字都表示的，这是一种所有符号的编码。

Unicode 为世界上所有字符都分配了一个唯一的数字编号，这个编号范围从 0x000000 到 0x10FFFF (十六进制)，有 110
多万，每个字符都有一个唯一的 Unicode 编号，这个编号一般写成 16 进制，在前面加上 U+。例如：

U+9A6C表示汉字马，

U+4E25表示汉字严

U+0639表示阿拉伯字母Ain，

U+0041表示英语的大写字母A，

Unicode 就相当于一张表，建立了字符与编号之间的联系

![](https://tva1.sinaimg.cn/large/006tNbRwgy1g9sw8x2yobj30em0m2aeg.jpg)

### 3.1 Unicode存在的问题

Unicode 只是一个符号集，它只规定了符号的二进制代码，却没有规定这个二进制代码应该如何存储。

比如，汉字严的 Unicode
是十六进制数4E25，转换成二进制数足足有15位（100111000100101），也就是说，这个符号的表示至少需要2个字节。表示其他更大的符号，可能需要3个字节或者4个字节，甚至更多。

这里就有两个严重的问题：

**问题一：** 如何才能区别 Unicode 和 ASCII ？计算机怎么知道三个字节表示一个符号，而不是分别表示三个符号呢？

**问题二：** 英文字母只用一个字节表示就够了，如果 Unicode
统一规定，每个符号用三个或四个字节表示，那么每个英文字母前都必然有二到三个字节是0，这对于存储来说是极大的浪费，文本文件的大小会因此大出二三倍

### 3.2 它们造成的结果是

**结果一：** 出现了 Unicode 的多种存储方式，也就是说有许多种不同的二进制格式，可以用来表示 Unicode，主要有
UTF-8，UTF-16，UTF-32。

**结果二：** Unicode 在很长一段时间内无法推广，直到互联网的出现

## 四 UTF-8

互联网的普及，强烈要求出现一种统一的编码方式。

UTF-8 就是在互联网上使用最广的一种 Unicode 的实现方式。

其他实现方式还包括 UTF-16（字符用两个字节或四个字节表示）和 UTF-32（字符用四个字节表示），不过在互联网上基本不用。

**注意：UTF-8 是 Unicode 的实现方式之一** 。

### 4.1 UTF-8 特点

UTF-8 最大的一个特点，就是它是一种变长的编码方式。它可以使用1~4个字节表示一个符号，根据不同的符号而变化字节长度。

### 4.2 UTF-8 的编码规则

UTF-8 的编码规则有二条：

**规则一：** 对于单字节的符号，字节的第一位设为0，后面7位为这个符号的 Unicode 码。因此对于英语字母，UTF-8 编码和 ASCII
码是相同的。

**规则二：** 对于n字节的符号（n > 1），第一个字节的前n位都设为1，第n +
1位设为0，后面字节的前两位一律设为10。剩下的没有提及的二进制位，全部为这个符号的 Unicode 码。  
下表总结了编码规则，字母x表示可用编码的位。

编号范围（编号对应十进制数） | 二进制格式  
---|---  
十六进制范围:(0x00---0x7f) 十进制范围:(0---127) | 0XXXXXXX  
十六进制范围:(0x80---0x7ff) 十进制范围:(128---2047) | 110XXXXX 10XXXXXX  
十六进制范围:(0x800---0xffff) 十进制范围:(2048---65535) | 1110XXXX 10XXXXXX 10XXXXXX  
十六进制范围:(0x10000---0x10ffff) 十进制范围:(65536以上) | 11110XXX 10XXXXXX 10XXXXXX
10XXXXXX  
  
## 五 Unicode和UTF-8之间的转换

跟据上表，解读 UTF-8
编码非常简单。如果一个字节的第一位是0，则这个字节单独就是一个字符；如果第一位是1，则连续有多少个1，就表示当前字符占用多少个字节。

下面，还是以汉字严为例，演示如何实现 UTF-8 编码。

严的 Unicode 是4E25（100111000100101），根据上表，可以发现4E25处在第三行的范围内（0000 0800 - 0000
FFFF），因此严的 UTF-8 编码需要三个字节，即格式是1110xxxx 10xxxxxx
10xxxxxx。然后，从严的最后一个二进制位开始，依次从后向前填入格式中的x，多出的位补0。这样就得到了，严的 UTF-8 编码是11100100
10111000 10100101，转换成十六进制就是E4B8A5

