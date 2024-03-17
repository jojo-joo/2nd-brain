### 概念

动态 NFC 标签的天线通常是放置于 PCB 电路板上的一圈**环形走线**，其阻抗与内部的**调谐电容值**相匹配，进而创建出一个 `13.56MHz` 频率的共振电路，这个调谐频率的基本方程为:

$
f_{tune}=\frac{1,3}
$
## 原理

将 NFC 标签放置于 NFC 读卡器的电磁场当中，NFC 标签芯片的内置电路就会开始**解调**来自于读卡器的信息：

![](media/v2-ee143525de7f4858fd858de51339d8d0_720w.webp.jpg)

==通信结束之后，NFC 读卡器会继续为 NFC 标签供电，以使得 NFC 标签产生一个应答响应，发送回 NFC 读卡器（动态 NFC 标签芯片会通过调节内部的输入阻抗，将应答信号反向散射到 NFC 读卡器）。==

![](media/v2-1f22158a159d98e9976c86730fbfb77e_720w.webp.jpg)

 整个数据通信过程所涉及的全部标准与协议，都已经嵌入到了 NFC 读卡器与 NFC 标签芯片的内部电路当中。


## 如何设计 PCB 天线

`13.56MHz` 频率下的 NFC 天线可以根据需要，被设计成为不同的形状。如前所述，需要重点关注的参数是天线在 `13.56MHz` 频率下的**等效电感** LA 。

> ▶ 杂散电容非常难进行近似的取值，通常处于以 `pF` 作为单位的范围以内。

接下来的内容会给出计算各种形状天线**自感** Lant 的有效公式（不考虑天线杂散电容），以及如何通过意法半导体官方提供的 [eDesignsuite](https://link.zhihu.com/?target=https%3A//www.st.com/content/st_com/en/support/resources/edesign.html) 工具来计算**等效电感** LA。

### 圆形天线的电感

![[Pasted image 20221109172316.png]]

### 螺旋形天线的电感

![[Pasted image 20221109172329.png]]

### 方形天线的电感

![[Pasted image 20221109172345.png]]
### eDesignSuite 天线设计工具

[eDesignSuite - Design, Circuit and Simulation Tools - STMicroelectronics](https://www.st.com/content/st_com/en/support/resources/edesign.html)
[NFC Inductance | RF Design | eDesignSuite | STMicroelectronics](https://eds.st.com/antenna/#/)


### PCB 布局

### 动态 NFC 标签芯片与天线之间的连接长度

动态 NFC 标签芯片必须尽可能的靠近天线（位于几毫米范围以内），任何额外的布线都会改变天线的特性。

### 地平面 电源 信号层

布局 PCB 上的 NFC 天线时，需要注意天线的顶层与底层两个面都不能进行铺铜，并且在天线周围也最好不要存在铜平面；下面的示意图展示了 NFC 天线的最优布局：**NFC 标签芯片靠近天线，而接地平面远离天线**。

![](media/v2-7a6d9f9009e3a6aa330c56a3d3d31b28_720w.webp)

## Reference
1. [How to design an antenna for dynamic NFC tags](assets/how-to-design-a-13-56-mhz-customized-antenna-for-st25-nfc-rfid-tags-stmicroelectronics.pdf)
2. [Measurement and tuning of a NFC and Reader IC antenna  with a MiniVNA](assets/Measurement%20and%20tuning%20of%20a%20NFC%20and%20Reader%20IC%20antenna%20%20with%20a%20MiniVNA.pdf)
3. [nRF52832 NFC Antenna Tuning.pdf](assets/nRF52832%20NFC%20Antenna%20Tuning.pdf)
