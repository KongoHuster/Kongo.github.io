---
title: PCIe简介
date: 2020-06-22 15:14:03
categories: UEFI
tags:
- UEFI
---

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="PCIe简介/neqwon.jpg">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px;">蓝天小屋</div>
</center>

#### 一、PCI-X总线基本概念

PCI-X总线在PCI总线的基础上发展而来，其在软件和硬件层面上都是兼容PCI总线的，但是却显著的提高了总线的性能。也就是说PCI-X的设备可以直接插到PCI的插槽中去，PCI的设备也可以直接插到PCI-X的插槽中去。

<!-- more -->

从硬件层面上来说，PCI-X继承了PCI总线中的Reflected-Wave Signaling，但是在信号的输入端加入了输入寄存器以增强时序性能，提高了总线的时钟频率。在PCI-X2.0的Spec中还提出了DDR和QDR技术，进一步提高了PCI-X总线的带宽。

一个典型的PCI-X总线系统的例子如下图所示：

![GkefGn.png](https://s1.ax1x.com/2020/03/28/GkefGn.png)

下面是一个PCI-X 突发读存储操作（Burst Memory Read Bus Cycle）的例子：

![GkeTqU.png](https://s1.ax1x.com/2020/03/28/GkeTqU.png)

在PCI总线中，以总线主机从从机设备读操作为例，当从机设备尚未准备好结束这次操作（从机设备未就绪，且数据尚未发送完）时，可以通过锁存数据并插入等待周期，或者发起Retry操作。PCI-X总线采用了一种叫做Split Transaction的方式来处理这种情况，如下图所示。此时，发起读操作的总线主机被称为Requester，而接受并向总线上发送数据的从机设备被称为Completer。

注：PCIe Spec中继承了PCI-X的这种命名方式。

![GkeIMV.png](https://s1.ax1x.com/2020/03/28/GkeIMV.png)

采用这种方式的PCI-X总线的总线传输利用率（效率）可以达到85%，而标准的PCI总线只有50%-60%。关于Split Transaction的详细内容，建议大家去参考PCI-X的Spec，这里不再详细地介绍。此外，PCI-X总线还在配置地址寄存器（Configuration Address Register）中加入了NS（No Snoop）和RO（Relaxed Ordering）两位以提高总线传输效率。

前面的文章中介绍过，PCI总线的中断操作是通过一系列的边带信号（Sideband Signals）来完成的，在PCI-X Spce中引入了消息信号中断（MSI，Message Signaled Interrupts）的机制，以取代这些边带信号，进而精简系统设计。


#### 二、PCIe简介

PCI-Express是继ISA和PCI总线之后的第三代I/O总线，即3GIO。 由Intel在2001年的IDF上提出，由PCI-SIG（PCI特殊兴趣组织）认证发布后才改名为“PCI-Express”。它的主要优势就是数据传输速率高，另外还有抗干扰能力强，传输距离远，功耗低等优点。 

注：第一代总线一般指ISA、EISA、VESA和Micro Platforms。第二代总线一般指PCI、AGP和PCI-X。

![GknVk4.png](https://s1.ax1x.com/2020/03/28/GknVk4.png)

图中的PCI-E的传输速率指的是实际的有效传输速率，为RAW Data速率的80%，因为PCI-E（Gen1&Gen2，Gen3中使用了新的方式，即128b/130b）中使用了8b/10b编解码技术。

PCI-Express总线的Spec中明确规定了PCI-Express的缩写为PCIe，但很多情况下，大家为了方便常把它缩写为PCI-E。

PCI-E接口根据总线位宽不同而有所差异，一个PCI Express连接可以被配置成x1， x2， x4， x8， x12， x16和x32的数据带宽。 (x2 and x12 link widths are optional) PCI-E各种位宽Device可以自由搭配使用，比如x1 的卡可以插到x8的插槽中使用， x8的卡可以插到x16的插槽中使用，升级方便。 

![GknA7F.png](https://s1.ax1x.com/2020/03/28/GknA7F.png)

一些常见的PCI-E设备如下图所示：


![Gknk0U.png](https://s1.ax1x.com/2020/03/28/Gknk0U.png)