---
{"dg-publish":true,"permalink":"/02-pages/IO控制器组成/","tags":["personal/blog","os","计算机组成原理"]}
---

如图所示：
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/08/20240805215821.png)

```ad-note
title: 小细节
1. 一个IO控制器可能会对应多个设备；
2. 数据寄存器、控制寄存器、状态寄存器可能会有多个，故而这些寄存器也要有相应的地址，CPU才能进行精准控制。当这些寄存器的编址占用内存地址的一部分时，就是[[02-pages/内存映射 IO\|内存映射 IO]]，如果采用IO专用地址就是[[02-pages/寄存器独立编址\|寄存器独立编址]]；
```

[[内存映射 IO]]
[[寄存器独立编址]]