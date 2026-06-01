# 计算机科学导论学习笔记
based 斯坦福大学Patrick Young教授主讲的计算机科学导论CS105:Introduction to Computer Science课程2023版。

## 字、字节与进制
常用进制：十进制(Decimal Number System, Base 10)、二进制(Binary Number System, Base 2)、八进制、十六进制。

### 二进制在计算机存储中
二进制仅有0，1两个数字，可以理解为一个开关的“关、开”两种情况，而这一个开关则视为1个字(Bit)。
8个为一组的开关，组成1字节(1 Byte = 8 Bits)。
实际物理存储对2进制的表达：
- SSD(Solid State Drive)：使用电子开关进行存储；
- HDD(Hard Disk Drive)：具有一片磁性极化区，通过判断磁向确认，同向为0，反向为1；
- CD、DVD、Blu-ray：物理凹坑和平坦示意1和0。

### 进制表达与转换
$$ 6_{10} = 110_{2} $$
上式中，10与2下标表示对应数字目前的进制，10进制中的6就是2进制中的110。
进制转换:
$$ 
    1891_{10} = 1 \times 10^{3} + 8 \times 10^{2} + 9 \times 10^{1} + 1 \times 10^{0} \\
    1101_{2} = 1 \times 2^{3} + 1 \times 2^{2} + 0 \times 2^{1} + 1 \times 2^{0} = 13_{10}
$$
通过这样表达数字的方式即可列式计算由2进制换算为10进制数。

### 字组合和存储容量表达
4 Bits能够在二进制中实现16种组合，即可以表达十进制中的0~15共16(=2^{4})个数字。
映射到 n Bits，既有$ 2^{n} $种组合。
存储容量表达：
$$ 
1 kilobyte(1KB) = 2^{10} Bits \\
1 megabyte(1MB) = 2^{20} Bits \\
1 gigabyte(1GB) = 2^{30} Bits \\
1 terabyte(1TB) = 2^{40} Bits \\
Peta = 2^{50},Exa = 2^{60},Zetta = 2^{70},Yotta = 2^{80}
$$



