---
{"dg-publish":true,"permalink":"/02-pages/Cache 替换算法-先进先出 FIFO/","tags":["personal/blog","计算机组成原理"]}
---

若 Cache 已满，则替换最先被调入的 Cache 的块。但是问题在于最先被调入的块也有可能是最频繁被访问，所以依然没有考虑[[02-pages/局部性原理\|局部性原理]]。

如下图：
**![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/09/20240904214921.png)
