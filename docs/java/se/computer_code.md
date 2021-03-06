# 计算机原码 反码 补码

[👈 **相关面试题**](./README.md#_37-能将-int-强制转换为-byte-类型的变量-如果该值大于-byte-类型的范围-将会出现什么现象)

## 机器数

一个数在计算机中的表现形式叫做机器数, 这个数有正负之分, 在计算机中用一个数的最高位（符号位）用来表示它的正负, 其中0表示正数, 1表示负数.

例如正数7, 在计算机中用一个8位的二进制数来表示, 是00000111, 而负数-7, 则用10000111表示, 这里的00000111和10000111是机器数

# 真数

计算机中的机器数对应的真实的值就是真数, 对最高位（符号位）后面的二进制数转换成10进制, 并根据最高位来确定这个数的正负.对于上面的00000111和10000111来说, 对最高位后面的二进制数转换成10进制是7, 在结合最高位的值, 得出对应的真数分别是7和-1

# 原码

用第一位表示符号, 其余位表示值.因为第一位是符号位, 所以8位二进制数的取值范围就是:[1111_1111 , 0111_1111]  即 [-127 , 127] ,原码是容易被人脑所理解的表达方式

# 反码

正数的补码反码是其本身, 负数的反码是符号位保持不变, 其余位取反.例如正数1的原码是[0000_0001], 它的反码是是其本身

[0000_0001],-1的原码是[1000_0001],其反码是[1111_1110]

# 补码

正数的补码是其本身, 负数的补码是在其反码的基础上+1, 例如正数1的原码是[0000_0001],他的补码是其本身[0000_0001],

-1的补码是[1111_1111]

# 有了原码为什么要使用反码和补码

因为人脑可以知道第一位是符号位, 可以根据符号位对真值的绝对值进行加减乘除, 但是对于计算机来说, 加减乘除是最最最基本的运算, 要设计的尽量简单, 计算机辨别符号位会让计算机的设计电路变得很复杂, 于是人们想出了让符号位也参与到运算上来.减去一个数, 等于加上他的负数.

使用原码参数运算的缺陷

![](./imgs/4548bac5.png)

从上面的原码表中可以看见左边每增加一个二进制单位对应的真数是递减的, 而右边每增加一个二进制单位对应的真数是递增的, 所以对于原码来说, 能满足正数的加法, 但无法满足负数的加法

2+1 = [0000_0010]原+[0000_0001]原=[0000_0011]原 = 3

1+-1=[0000_00001]原+[1000_0001]原=[1000_0010]原=-2

为了满足负数对加法的需求, 就必须让负数与他对应的二进制码是同步递增或者同步递减

于是就通过符号位不变, 其余位取反来满足这个同步递增或者递减的要求, 由于正数本来就满足它本身的加法, 所以不需要做任何改变.这就是反码的定义由来.

![](./imgs/87d2d2be.png)

从上图的反码表中可以看到在运算不跨过0的时候, 正负数的加法已经能满足要求

-2+1=[1111_1101]反+[0000_0001]反=[1111_1110]反=-1

127+1=[1000_0000]反=-127=128 加法算出来是128, 由于128超过最大值, 余1, 所以取最小值开始的第一位, 也就是

最小值-127, 但是这里有个不合理的地方, 就是[1111_1111]和[0000_0000]都表示0, 这导致在实际计算中每当跨过0一次, 就有一个单位的误差

-1+2=[1111_1110]反+[0000_0010]反=[0000_0000]反=0

要解决这个问题就必须让反码中的[1111_1111]和[0000_0000]合并, 

由于[1111_1111]+[0000_0001]=[0000_0000],所以在负数反码的基础上+1就可以解决反码中跨0的误差问题, 同时不会对负数与它对应的二进制反码的同步递增产生影响, 所以在反码的基础上+1就完美的解决了符号参与预算的问题, 这就是补码为什么是在负数反码的基础上+1的由来.

![](./imgs/39245ce8.png)

从上面的图中发现还有一个[1000_0000]的二进制没有对应任何真数, 于是就规定了这个数的真数是-128

所以补码的表示范围是[-128~127] , 这样一来256个二进制正好表示256个整数, 在实际二进制的运算中超过范围其实就是对256的取余预算（x+128）mod 256 - 128.

[👈 **返回面试题**](./README.md#_37-能将-int-强制转换为-byte-类型的变量-如果该值大于-byte-类型的范围-将会出现什么现象)
