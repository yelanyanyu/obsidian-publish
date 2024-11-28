---
{"dg-publish":true,"permalink":"/02-pages/CPU 的基本结构/","tags":["personal/blog","计算机组成原理/CPU"]}
---

- [[02-pages/控制器的基本结构\|控制器的基本结构]]；
- [[02-pages/运算器的基本结构\|运算器的基本结构]]；

![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/11/20241121220224.png)
```ad-note
title: 解读
1. 控制器和运算器通过 CPU 内部总线连接起来，进行数据的交互。
2. 核心的组件就是 ALU 和 微操作信号发生器。例如，在执行完一条指令后，PC 要加 1，那么就由微操作信号发生器控制 PC out ，然后让这个数据流通至 ALU 执行 +1 的操作，随后送回 PC；
```


![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/11/20241121222814.png)
