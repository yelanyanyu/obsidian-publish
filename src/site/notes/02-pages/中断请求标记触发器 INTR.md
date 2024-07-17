---
{"dg-publish":true,"permalink":"/02-pages/中断请求标记触发器 INTR/","tags":["personal/blog","计算机组成原理","概念"]}
---

中断系统需对每个中断源设置 INTR，当其状态为 1 时，表示该中断源有请求。这些触发器可组成中断请求标记寄存器，该寄存器可集中在 CPU 中，也可分散在各个中断源中。如下图：
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/07/20240712211643.png)
