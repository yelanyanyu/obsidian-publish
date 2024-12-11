---
{"dg-publish":true,"permalink":"/02-pages/ALU/","tags":["personal/blog","hardware","计算机组成原理/CPU"]}
---

```ad-info
是[[02-pages/运算器\|运算器]]的核心组件，实现了加/减/乘/除、与/或/非 等功能。但是加减乘除等运算都是基于 “加法” 来实现的，故而加法器是 ALU 的核心。ALU 的内部结构示意图为：
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/12/20241210214557.png)

```
[[运算器]]、
在实际考试中，我们不需要看懂内部的具体原理。只需要理解以下的图即可：
![image.png|300](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/12/20241210214714.png)

```ad-note
title: 解读
- ZF/OF/SF/CF 是标志位，用来表示此次运算结果的状态，这些标志信息通常会被送入 [[程序状态字寄存器|PSW]] 中。
- op 是控制信号，用来指示 ALU 此时应该怎么运作，所以如果 ALU 支持 k 种功能，那么控制信号的位数至少为 $\displaystyle m\geq \lceil \log_{2}k \rceil$。
```
