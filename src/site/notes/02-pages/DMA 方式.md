---
{"dg-publish":true,"permalink":"/02-pages/DMA 方式/","tags":["personal/blog","计算机组成原理"]}
---

DMA 方式是一种**完全由硬件进行成组信息传送的控制方式**，它具有[[02-pages/程序中断方式\|程序中断方式]]的优点，即在数据准备阶段，CPU 与外设并行工作。

在 DMA 方式中，中断的作用仅限于故障和正常传送结束时的处理。

[[主存\|主存]]和 DMA 接口之间有一条直接数据通路。由于 DMA 方式传送数据不需要经过 CPU, 因此不必中断现行程序，I/O 与主机并行工作，程序和传送并行工作。

DMA 方式的核心硬件就是 [[02-pages/DMA 控制器\|DMA 控制器]]，如下图，就是 DMA 与其他构件的关系。
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/07/20240715105330.png)
+ 接受外设发出的 DMA 请求（外设传送一个字的请求），并向 CPU**发出总线请求**。
+ CPU 响应此总线请求，发出总线响应信号，接管总线控制权，进入DMA操作周期。
+ 确定传送数据的主存单元地址及长度，并能自动修改主存地址计数和传送长度计数。
+ 规定数据在主存和外设间的传送方向，发出读写等控制信号，执行数据传送操作。
+ 向 CPU 报告 DMA 操作的结束。   


与程序中断方式的对比：
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/07/20240715105431.png)
