---
{"dg-publish":true,"permalink":"/02-pages/Cache 和 主存的映射方式/","tags":["personal/blog","计算机组成原理"]}
---

显然，Cache 要做到转存主存的数据，需要有一个**有效位**，用来表示这个块是否已经有内容，也需要一个**标记**，用来表示存放的是主存是哪里的数据？

![image.png|600](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/09/20240904211022.png)
```ad-note
title: 解读
为了更好的存放数据，我们将各个相邻的存储单元划分为一个组，统一管理。这个组称为块。一般块的大小都为 1 KB。

而**主存块的大小往往与 Cache 的块大小相同**。
```

有三种常用的映射方式：
 - [[02-pages/全相联映射\|全相联映射]]；
 - [[02-pages/直接映射\|直接映射]]；
 - [[02-pages/组相联映射\|组相联映射]]；


![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/09/20240904212346.png)
