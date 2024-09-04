---
{"dg-publish":true,"permalink":"/02-pages/Cache 的写策略/","tags":["personal/blog","计算机组成原理"]}
---

当 CPU 了修改了 Cache 中的数据后，需要将修改同步到主存中，即保持主存中数据母本的一致性。

- 当写命中时的两种方法：
	- [[02-pages/全写法-写直通法\|全写法-写直通法]]；
	- [[02-pages/写回法\|写回法]]；
- 写不命中时的方法（即Cache 块中没有数据，需要从主存调入时）：
	- [[02-pages/写分配法\|写分配法]]；
	- [[02-pages/非写分配法\|非写分配法]]；
- [[02-pages/多级 Cache 的写策略\|多级 Cache 的写策略]]；

![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/09/20240904222229.png)
