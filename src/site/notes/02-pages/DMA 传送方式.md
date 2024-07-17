---
{"dg-publish":true,"permalink":"/02-pages/DMA 传送方式/","tags":["personal/blog","计算机组成原理"]}
---

[[主存\|主存]]和 DMA 控制器之间有一条数据通路，因此主存和 IO 设备之间交换信息时，不通过 CPU。但当 IO 设备和 CPU 同时访问主存时，可能发生冲突，为了有效地使用主存，DMA 控制器与 CPU 通常采用以下三种方式使用主存。
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/07/20240715105413.png)
+ 停止 CPU 访存方式；
+ DMA 与 CPU 交替访存方式：不能很好的利用 CPU 的效率，因为 CPU 的访存可能很频繁，而 DMA 控制器的访存大部分时候远远没有 CPU 频繁。
+ 周期挪用：也称为循环挪用方式。因为 DMA 数据寄存器大小有限，所以，我们需要有限让 IO 访存优先。


```ad-note
title: 注意
周期挪用方式是<font color="#ff0000">单字传送方式</font>，挪用一个存取周期，传送完一个数据字后立即释放总线。
```
