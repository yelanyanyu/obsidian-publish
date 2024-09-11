---
{"dg-publish":true,"permalink":"/02-pages/Cache 命中率规律/","tags":["personal/blog","计算机组成原理"]}
---

![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/09/20240908220505.png)
这种增加趋势在某一个最佳块大小处达到最大值。当到达顶峰以后，命中率随着块大小的增加反而会逐渐降低。因为当块大小非常大时，进入 Cache 中的许多数据可能根本用不上，同时随着块继续增大，时间局部性会逐渐失去作用。