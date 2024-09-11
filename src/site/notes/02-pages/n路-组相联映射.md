---
{"dg-publish":true,"permalink":"/02-pages/n路-组相联映射/","tags":["personal/blog","计算机组成原理"]}
---

综合了[[02-pages/全相联映射\|全相联映射]] 和[[02-pages/直接映射\|直接映射]]的思想，将 Cache 块分为若干组，每个主存块可放到特定分组中的任意一个位置。若每个分组内有 n 块 Cache line ，则称这个 Cache 的映射方式为 **n 路组相联映射**。即：
```ad-note
组号 = 主存块号 % 分组数
```

为了实现这个功能，我们需要在主存块号中选择几位作为组号。这个组号的占用位数肯定要小于[[02-pages/直接映射\|直接映射]]的。

例如：
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/09/20240904212313.png)

可见在分组的选择上，我们采用了直接映射，在分组内块的选择上，我们选了全相联映射的思想。

当分组满了，需要选择分组内的哪一块进行替换？[[02-pages/Cache 替换算法\|Cache 替换算法]]。