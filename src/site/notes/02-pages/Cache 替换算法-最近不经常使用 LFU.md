---
{"dg-publish":true,"permalink":"/02-pages/Cache 替换算法-最近不经常使用 LFU/","tags":["personal/blog","计算机组成原理"]}
---

设置计数器，记录每个 Cache 块被访问过几次。当 Cache 满后，替换计数器最小的。

如下图：
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/09/20240904220854.png)


曾经被访问的主存块在未来不一定会用到（如：微信聊天相关的块），并没有很好的遵循[[02-pages/局部性原理\|局部性原理]]，所以实际效果不如 [[02-pages/Cache 替换算法-近期最少使用 LRU\|Cache 替换算法-近期最少使用 LRU]]。