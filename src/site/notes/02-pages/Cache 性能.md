---
{"dg-publish":true,"permalink":"/02-pages/Cache 性能/","tags":["personal/blog","计算机组成原理"]}
---

为了衡量 Cache 的性能，需要引入几个概念：
 - 命中率 $\displaystyle H$；
 - 未命中率 $\displaystyle M=1-H$；
显然，H 越高，效率越高。

设 $\displaystyle t_{c}$ 为访问一个 Cache 的时间，$\displaystyle t_{m}$ 为访问一次主存所需的时间。当 CPU 用不同的方式访问 Cache 的性能当然也是不同的，有两种不同的访问方式，我们分别计算其平均访问时间 $t$：
 1. 先访问 Cache，若 Cache 未命中再访问主存；
	 $\displaystyle t=Ht_{c}+(1-H)(t_{c}+t_{m})$。
 2. 同时访问 Cache 和主存，若 Cache 命中则立即停止访问主存：
	 $\displaystyle t=Ht_{c}+(1-H)t_{m}$。

![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/09/20240903223309.png)

