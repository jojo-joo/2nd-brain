## Overview

Arm Mbed DAPLink is an open-source software project that enables programming and debugging application software on running on Arm Cortex CPUs. Commonly referred to as interface firmware, DAPLink runs on a secondary MCU that is attached to the SWD or JTAG port of the application MCU. This configuration is found on nearly all development boards. It creates a bridge between your development computer and the CPU debug access port. DAPLink enables developers with drag-and-drop programming, a serial port and CMSIS-DAP based debugging. More features are planned and will show up gradually over time. The project is constantly under heavy development by Arm, its partners, numerous hardware vendors and the open-source community around the world. DAPLink has superseded the mbed CMSIS-DAP interface firmware project. You are free to use and contribute. Enjoy!

It’s in the [Arm Mbed GitHub organization](https://github.com/armmbed/DAPLink). There you can make feature requests and file bugs about the interface firmware or release site.

## Setup

DAPLink sources can be compiled using Keil MDK-ARM or mbed cli tool with arm compiler, which could be run both on Linux and Windows. See  [here](AUTOMATED_TESTS.md) for test instructions on both OS and Mac.

Install the necessary tools listed below. Skip any step where a compatible tool already exists.

* Install [Python 2, 2.7.11 or above](https://www.python.org/downloads/) . Add to PATH.
* Install [Git](https://git-scm.com/downloads) . Add to PATH.
* Install [Keil MDK-ARM](https://www.keil.com/download/product/), preferably version 5. Set environment variable "UV4" to
  the absolute path of the UV4 executable if you don't install to the default location. Note that "UV4" is what's used for
  both MDK versions 4 and 5. This step can be skipped if you plan to use mbed cli, but you still need Arm Compiler 5, and
  MDK is required to debug.
* Install virtualenv in your global Python installation eg: `pip install virtualenv`.

> [!NOTE]
> - 必须是python2才能成功(经测试python3不成功)
> - 没有装git需要改配置改代码

### **Step 1.** Initial setup.

Get the sources and create a virtual environment

```shell
$ git clone https://github.com/mbedmicro/DAPLink
$ cd DAPLink
$ pip install virtualenv
$ virtualenv venv
```

> [!NOTE]
> if can not connect to the offical site, please change the [[pip source ]]

### **Step 2.** Activate the virtual environment and update requirements.

 This is necessary when you open a new shell. **This should be done every time you pull new changes**

```shell
$ venv/Scripts/activate   (For Linux)
$ venv\Scripts\activate.bat   (For Windows)
$ pip install -r requirements.txt
```

> [!NOTE]
> if can not connect to the offical site, please change the [[pip source ]]
> ERROR: No matching distribution found for 如果遇到这种错误请用下面指令升级PIP
> **python -m pip install --upgrade pip**

## Build

**This should be done every time you pull new changes**

There are two ways to build DAPLink. You can generate Keil MDK project files and build within MDK. MDK is also used to debug DAPLink running on the interface chip. Or, you can use the `mbedcli_compile.py` script to build projects from the command line without requiring MDK.

### **Step 3.** For MDK progen compilation.

This command generates MDK project files under the `projectfiles/uvision` directory.
```shell
$ progen generate -t uvision
```

To only generate one specific project, use a command like this:
```shell
progen generate -f projects.yaml -p stm32f103xb_stm32f746zg_if -t uvision
```
These options to `progen` set the parameters:
- `-f` for the input projects file
- `-p` for the project name
- `-t` to specify the IDE name 

![[Pasted image 20221019232153.png]]

> [!NOTE]
> 请忽略如上所示警告

### **Step 4.** Build Enviroment

enter projectfiles\uvision directory, you will see lots of projects 
![[Pasted image 20221019235902.png]]
we only concern on stm32f103xb_bl and stm32f103xb_stm32f103rb_if projects
如果没有安装git,那么下面的红色框就不能勾选
![[Pasted image 20221020000441.png]]

### **Step 5.** Build stm32f103xb_bl

这个是BootLoad 程序，就是支持固件下载所必须的，**后期需要使用拖动下载的方法下载daplink的固件**。是需要通过另一个下载器烧写的。同时运行的固件，可以通过直接烧录好的 Bootload 进行更新。打开工程。
编译成功之后生成hex文件，
如果没有安装git,那么下面的红色框就不能勾选

![[Pasted image 20221020000441.png]]

### **Step 6.** Build stm32f103xb_stm32f103rb_if

是因为我使用的是stm32f103c8t6，这款mcu与stm32f103cbt6为同型号单片机，具体的硬件资源都是一样的中容量mcu，区别不同是，stm32f103c8t6的内部flash为64kb，xb为128kb。
但是经过实际测试，c8t6完全可以使用，不存在flash不够用的情况
打开对应工程，编译得到hex文件。

## Download

### **Step 7.** 下载stm32f103xb_bl.hex

用JLINK或者STLink下载 stm32f103xb_bl.hex到单片机
we can see a new disk named "MAINTENANCE"

### **Step 8.**  rst重启得到U盘

下载完成后， 将烧录完 BL 的下载器的“RST” 端口短接到 GND 后重新上电插入电脑 USB，此时电脑会枚举出一个 U 盘， 如图。
![[Pasted image 20221020001240.png]]

### **Step 9.**  升级到DAPLINK

 - 去掉 RST 到 gnd 的跳线
 - 拖动stm32f103xb_stm32f103rb_if.hex到U盘
 - 将RST跳线连接到3.3V
 - 重新拔插DAPLINK
 - 此时电脑就会重新识别出来一个优盘
![[Pasted image 20221020001555.png]]

注意U盘里面有个文件叫==DETAILS.TXT==,里面有个变量叫==Daplink Mode==,可以代表当前是升级模式还是调试模式,如下图
![[Pasted image 20221022184744.png]]

### **Step 10.**  keil
连接单片机，在keil的debug串口会显示对应的芯片，这时候点击下载即可。

![[Pasted image 20221020001703.png]]
 这个时候 SW Device 里面是空的, 将目标板子连接好后,变成
 ![[Pasted image 20221022185353.png]]
 DAPLINK可以烧录调试了
 
### **Step 11.**  Segger Studio

- 解压缩openocd-v0.12.0-rc1.7z到一个文件夹,最好没有特殊字符和中文字符.
- 双击adp.bat
- 打开SES,第一次会出一个报警,请勾选并ACCEPT
- Debug-->Target Connection-->GDB Server
  ![[Pasted image 20221022202551.png]]
- GDB Server-->Type-->OpenOCD
![[Pasted image 20221022202806.png]]
