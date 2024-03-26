最近要把巴特沃斯滤波算法移植到嵌入式端，之前一直是在服务器端用python搞的，简单粗暴，几行代码就解决问题了，如下：

```jupyter
import numpy as np
from scipy import signal

def data_filt(array):
    array = array-np.mean(array, axis=1, keepdims=True)
    b, a = signal.butter(4, [0.5, 30], 'bandpass',fs=Fs) 
    print('b = ', b)
    print('a = ', a)
    data = signal.filtfilt(b, a, array, axis=1)
    return data
```


采用`scipy.signal`里面的`butter函数`，先计算滤波系数`b`,`a`,然后利用`signal.filtfilt`进行**iir滤波**。

效果展示如下，输入一个采样率为250Hz的信号，进行[0.5 30]Hz范围内的带通滤波。

  

![](media/14008760-41f81883943aabbf.png.webp)

butter带通滤波

可以看出，对高频分量和低频分量都进行了有效滤除，效果还不错。

总结下总共两步：

-   **计算butter系数b，a**
-   **将b，a输入到iir滤波器，进行滤波**

其中iir滤波的公式如下：  
![[Pasted image 20221129130637.png]]

现在要做的是，把这两步用c实现。

### 1. 计算butter系数

找了很多教程，最后发现了一个神奇的网站，[https://www-users.cs.york.ac.uk/~fisher/mkfilter/trad.html](https://links.jianshu.com/go?to=https%3A%2F%2Fwww-users.cs.york.ac.uk%2F%7Efisher%2Fmkfilter%2Ftrad.html)，可以直接帮助计算参数b，a,且给出滤波代码。  

![](media/14008760-133e330be27a5d94.png.webp)

自动计算滤波参数

  
如上图，把参数填好后，直接点击网页下方的提交，滤波系数就生成了,如下：

![](media/14008760-c93055fd9fc69853.png.webp)

结果展示

  
其中参数**b,a**就隐藏在Recurrence relation里面。根据公式![[Pasted image 20221129130637.png]]
可以知道，

```c
y[n] = (  1 * x[n- 8])
     + (  0 * x[n- 7])
     + ( -4 * x[n- 6])
     + (  0 * x[n- 5])
     + (  6 * x[n- 4])
     + (  0 * x[n- 3])
     + ( -4 * x[n- 2])
     + (  0 * x[n- 1])
     + (  1 * x[n- 0])

     + ( -0.1364651827 * y[n- 8])
     + (  1.3458282682 * y[n- 7])
     + ( -5.8802636583 * y[n- 6])
     + ( 14.8469602185 * y[n- 5])
     + (-23.7209182114 * y[n- 4])
     + ( 24.5619000249 * y[n- 3])
     + (-16.0672806050 * y[n- 2])
     + (  6.0502391423 * y[n- 1])
```

下面是C语言生成的系数
``` C
b=[1,0,-4,0,6,0,-4,0,1]
a =[1,-6.0502391423,16.0672806050,-24.5619000249,23.7209182114, -14.8469602185,5.8802636583,-1.3458282682,0.1364651827]
```

下面是PYTHON生成的系数
```python
b = [ 0.00842863 0. -0.03371451 0. 0.05057177 0.  -0.03371451 0. 0.00842863]  
a = [ 1. -6.05023914 16.06728061 -24.56190002 23.72091821  -14.84696022 5.88026366 -1.34582827 0.13646518]
```

> [!NOTE] 注意
> - 系数数组a的第一个成员1是y[n]的系数
> - a是一致的，但是b不一致，原因是因为他们之间**差了一个gain**。就是**Results**那副图里面的 **gain at centre: mag = 1.185137516e+02**, **b的每个值都除以1.185137516e+02，就和python的结果一致了**。




### 2. 将参数b，a用于iir滤波

 
比较牛逼的点就是，上面推荐的那个网站把这个也给做好了，我们基本上只需要复制。  

![](media/14008760-cc5e3858a412d436.png.webp)


```c
#define NZEROS 8
#define NPOLES 8
#define GAIN   1.185137516e+02

static float xv[NZEROS+1], yv[NPOLES+1];

static void filterloop()
  { for (;;)
      { xv[0] = xv[1]; xv[1] = xv[2]; xv[2] = xv[3]; xv[3] = xv[4]; xv[4] = xv[5]; xv[5] = xv[6]; xv[6] = xv[7]; xv[7] = xv[8]; 
        xv[8] = next input value / GAIN;
        yv[0] = yv[1]; yv[1] = yv[2]; yv[2] = yv[3]; yv[3] = yv[4]; yv[4] = yv[5]; yv[5] = yv[6]; yv[6] = yv[7]; yv[7] = yv[8]; 
        yv[8] =   (xv[0] + xv[8]) - 4 * (xv[2] + xv[6]) + 6 * xv[4]
                     + ( -0.1364651827 * yv[0]) + (  1.3458282682 * yv[1])
                     + ( -5.8802636583 * yv[2]) + ( 14.8469602180 * yv[3])
                     + (-23.7209182110 * yv[4]) + ( 24.5619000250 * yv[5])
                     + (-16.0672806050 * yv[6]) + (  6.0502391423 * yv[7]);
        next output value = yv[8];
      }
  }
```

利用这个代码，对信号进行滤波，并同python的结果对比，如下：

![](media/14008760-9aaddc06d9f49594.png.webp)

butter滤波对比。第一行是原始信号，第二行用python的butter代码滤波，第三行c的butter代码滤波

发现效果基本上都有了，但是还是差一点点，仔细看还是没有python滤波的效果好。  
**那么问题出在了那里呢？**

第一步计算参数b,a，python和c的结果是一样的。所以只能是出在了第二步：iir滤波。

python用到的是**signal.filtfilt()**函数。我们看下它的源码的核心部分：  

![](media/14008760-38af4491f481da3d.png.webp)

image.png

原因找到了，在filtfilt函数里，进行完Forward filter滤波——lfilter以后，把结果y做了翻转，然后又进行了一次lfilter滤波——即Backword滤波。最后再把y翻转回来，进行return。

相比较的话，我们的代码里只进行了Forward滤波，没有Backword。所以我们仿照这个方法，**把生成的结果y进行翻转，再做一次相同的滤波，最后再翻转回来，作为最终的滤波结果。**对照图如下：  

![](media/14008760-0d10cdc8ec310e81.png.webp)

对照图

YES!! 这一次python和c的结果就一模一样了。大功告成，完美！