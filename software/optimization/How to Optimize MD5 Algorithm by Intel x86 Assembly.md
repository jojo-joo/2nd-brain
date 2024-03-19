## Background
开始我们作个实验，先打开两个文档附带的程序，一个工程是MD5C,一个工程是MD5ASM,其中MD5C是从VCKBASE下载的md5算法的标准C语言原代码，MD5ASM是我修改后的md5算法原代码。我给这两个工程的main函数都里面添加了一段回朔代码，用来产生产生0～99999999的数字，然后用这两个工程里面的可执行文件去对每个数字md5加密。好了，经过一段时间的等待后，就可以看到类似的结果了：

MD5ASM工程在我的机器上的结果是181秒，MD5C在我的机器上产生的结果是999秒，呵呵，数字有点怪，不过我看了表的，差不多是这个时间，巨大的差距是怎样产生的，让我们接下来往下看吧。

在开始正题之前，大家需要清楚一件事，就是MD5C里面的代码虽然效率不高，但绝对是优秀的，因为它主要在演示md5的算法，用的是纯粹的C，没有添加任何平台相干的代码，而我改写的MD5ASM是只能够运行于x86上的windows系统中。所以速度是以兼容性来交换的。

## 算法优化
先观察一下MD5C里面的一段代码：
``` c
static void Encode (unsigned char *output, unsigned int  *input, unsigned int len)
{
    unsigned int i, j;

    for (i = 0, j = 0; j < len; i++, j += 4) {
        output[j] = (unsigned char)(input[i] & 0xff);
        output[j+1] = (unsigned char)((input[i] >> 8) & 0xff);
        output[j+2] = (unsigned char)((input[i] >> 16) & 0xff);
        output[j+3] = (unsigned char)((input[i] >> 24) & 0xff);
    }
}
```
 

这是一段将整数数组转换成为字符数组的代码，我们看看它到底做了些什么。假设主函数输入了一个整数0x30313233,那么这个子函数的调用就可以写成下面的样子：

``` c
    Encode (output, input, 1)
```

Input指向一个整数数组，数组的第一个元素是0x30313233，我们接下来看函数转换
i=0,j=0
``` c
    output[0]= (unsigned char)(input[0]& 0xff)=0x33
    output[1]= (unsigned char)(input[0]& 0xff)=0x32
    output[2]= (unsigned char)(input[0]& 0xff)=0x31
    output[3]= (unsigned char)(input[0]& 0xff)=0x30
```
i=0,j=4
    跳出循环
    output的内存排列顺序为
```
   +--+--+--+--+--
   |33|32|31|30|
   +--+--+--+--+--
   ^
output
```

现在大家注意了，input的排列顺序是什么？由计算机原理可知道，在计算机内部，数据的存放顺序是“高位对应高位，低位对应低位”，0x30313233中的33因为是个位，是低位，所以对应内存单元的最低位，同理30在内存单元的最高位，由此推出0x30313233在数组中的排列顺序为：

```
    +--+--+--+--+--
    |33 32 31 30|
    +--+--+--+--+--
    ^
input
```

结果显而易见了，这个函数的功能只是将一个无符号整形数组转换成为了一个无符号字符形数组，作者的目的我虽然不清楚，但是这个地方确实可以优化如下：
```
    output=(unsigned char *)input;
```
    把这个地方叫作算法的优化可能有点牵强，但是算法的优化确实是最为重要的，比如说搜索算法，如果选择不当，可能要丧失很多的效率。

## 内存拷贝优化

再观察一下MD5C里面的一段代码：
``` c
static void MD5_memcpy (unsigned char *output, unsigned int  *input, unsigned int len)
{
  unsigned int i;
  for (i = 0; i < len; i++)
    output[i] = input[i];
}
```

这处的为什么要修改是非常明显的，for循环是非常慢的，我们一般可以把类似的代码替换成为C的库函数或者操作系统的标准函数，如`memcpy`

这种内存代码你也千万不要尝试自己去实现，那将是一种灾难，在每个操作系统中，内存拷贝可以说是非常频繁的，所以系统的内存拷贝函数基本上都是非常完美的，不信的话你可以自己写一段内存拷贝函数，然后和系统的内存拷贝函数比较一下就知道了，具体原代码可以参考linux中string.lib的实现。

这处代码是特别值得注意的地方，如果你在你的代码的运算密集处写出了类似MD5_memcpy的代码的化，那么性能将会急剧的下降，对你的系统将是灾难性的。

## 使用汇编优化

下面又一段MD5C里面的代码，这种代码每一次md5加密要运算64次，而我们要穷举8位数字的话，就需要运算64000000次，将下面的代码用汇编展开以后大概是23行的汇编代码，也就是这个转换将耗费147200000000,即1472亿个指令周期。
``` c
#define UINT4 unsigned int
#define ROTATE_LEFT(x, n) (((x) << (n)) | ((x) >> (32-(n))))
#define F(x, y, z) (((x) & (y)) | ((~x) & (z)))
#define FF(a, b, c, d, x, s, ac) { /
 (a) += F ((b), (c), (d)) + (x) + (UINT4)(ac); /
 (a) = ROTATE_LEFT ((a), (s)); /
 (a) += (b); /
  }

```

### 减少内存的访问次数

把test1的代码反汇编得到代码如下：
``` Assembly
mov     eax,dword ptr [ebp-8]
and     eax,dword ptr [ebp-0Ch]
mov     ecx,dword ptr [ebp-8]
not     ecx                         
and     ecx,dword ptr [ebp-10h]
or      eax,ecx                     
add     eax,dword ptr [ebp-14h]
add     eax,dword ptr [ebp-1Ch]
mov     edx,dword ptr [ebp-4]
add     edx,eax                     
mov     dword ptr [ebp-4],edx
mov     eax,dword ptr [ebp-4]
mov     ecx,dword ptr [ebp-18h]
shl     eax,cl                      
mov     ecx,20h                    
sub     ecx,dword ptr [ebp-18h]
mov     edx,dword ptr [ebp-4]
shr     edx,cl                      
or      eax,edx                     
mov     dword ptr [ebp-4],eax
mov     eax,dword ptr [ebp-4]
add     eax,dword ptr [ebp-8]
mov     dword ptr [ebp-4],eax
```
 

从上面的代码可以看到，23条编译后的汇编代码中有16条是涉及内存访问指令，考虑到逻辑运算和四则运算的平均指令周期是4，而内存访问指令平均指令周期是10，所以上叙的内存访问指令将消耗整个程序时间的七成以上。所以，个人认为汇编的优化，其实质就是减少内存的访问，能够在寄存器内部完成的就不要反复的访问内存，此处我采取强制定义的方式使得这段程序基本不访问内存。

考虑到 FF(a, b, c, d, x, s, ac) 中s，ac是常数，x是一个数组，不好强制定义成寄存器，所以这里只强制定义了a,b,c,d四个变量为寄存器，以及tmp1，tmp2两个寄存器类型的临时变量。
``` Assembly
#define a esi
#define b edi
#define c edx
#define d ebx
#define tmp1 eax
#define tmp2 ecx
```

### 单句的优化

现在我们首先将上叙C代码用最简单朴实的思路转换成为汇编,其中ROTATE_LEFT(x, n)实际上是一个循环左移，可以替换成为 ROL，加减乘除也可以用汇编相应的代码来替换，开始要注意的是，考虑到中间的一些参数，比如x，a要反复利用，我们必须把结果用临时变量来保存，单句的优化如下：

``` Assembly
#define FF(a, b, c, d, x, s, ac)/
    __asm mov tmp1,b/
    __asm and tmp1,c/
    __asm mov tmp2,b/
    __asm not tmp2/
    __asm and tmp2,d/
    __asm or tmp2,tmp1/     // 以上为F(x, y, z)实现
    __asm add tmp2,x/
    __asm add tmp2,ac/
    __asm add a,tmp2/                  
    __asm rol a,s/          // (a) = ROTATE_LEFT(x, n)
    __asm add a,b/          // (a) += (b)
                            // 以上为FF(a, b, c, d, x, s, ac)实现
```

### 多句的整合优化

上面的代码已经从23行下降到了11行，是不是到了极限了呢。还没有，我们还可以作一些汇编行与行之间的优化，注意
``` Assembly
    __asm add tmp2,ac
    __asm add a,tmp2
```

此处，我们可以采取连加的语句来实现，大家可能回感到奇怪，汇编中有连加的语句吗，好像没有听说过啊，其实此处优化我们利用的是语句lea,大家看看下面的lea语句：
``` Assembly
    __asm lea a,[tmp2+a+ac]
```

lea的标准用法是：
``` Assembly
    LEA        REG，[BASE+INDEX*SCALE+DISP]
```

其实这种寻址方式常常用于数据结构中访问特定元素内的一个字段，base为数组的起始地址，scale为每个元素的大小，index为下标。如果数组元素始数据结构，则disp为具体字段在结构中的位移。此处我们是将scale设置为1，所以就变成了：
``` Assembly
    LEA        REG，[BASE+INDEX +DISP]
```

因此就实现了三个数字的连加，类似的优化方法还由不少
所以再次优化后如下共10句：

``` Assembly
#define FF(a, b, c, d, x, s, ac) /
    __asm mov tmp1,b /
    __asm and tmp1,c /
    __asm mov tmp2,b /
    __asm not tmp2 /
    __asm and tmp2,d /
    __asm or tmp2,tmp1 /
    __asm lea a,[tmp2+a+ac] /
    __asm add a,x /
    __asm rol a,s /
    __asm add a,b /
 ```

### 利用mmx指令优化

我的这几个工程虽然没有涉及到利用mmx指令优化，但是我们考虑到mmx指令是基于64位的，可以大大的减少运算量，最明显的例子是des算法，它加密的明文是64位，加密后的密文也是64位，所以使用mmx的寄存器mm0,mm1,……，可以得到性能的提升，不过要注意如何尽可能的满足U,V流水线的需求，就不在本文的讨论范围内了。
 