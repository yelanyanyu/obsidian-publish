---
{"dg-publish":true,"permalink":"/02-pages/IO控制器/","tags":["personal/blog","计算机组成原理","os"]}
---

CPU 无法直接控制 IO 设备的机械部件，所以需要一个中介，这个中介就是 IO 控制器（设备控制器）。CPU 控制 IO 控制器，然后 IO 控制器控制设备的机械部件。

- IO 控制器的功能：
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/08/20240805215641.png)
- [[02-pages/IO控制器组成\|IO控制器组成]]；
- 两种寄存器编址方式：
	- [[02-pages/内存映射 IO\|内存映射 IO]]；
	- [[02-pages/寄存器独立编址\|寄存器独立编址]]；
- 常见 IO 控制器：
	- [[02-pages/DMA 控制器\|DMA 控制器]]；