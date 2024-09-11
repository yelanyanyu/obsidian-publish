---
{"dg-publish":true,"permalink":"/02-pages/Cache 寻址过程/","tags":["personal/blog","计算机组成原理"]}
---

以 8 路组相联为例：
当 CPU 发出内存访问请求的时候，我们会得到一个内存地址，我们需要挑选主存地址中有效部分：
 1. 先把这个内存地址划分为 `tag, set index, block offset`；
 2. 根据 `set index` 找到组；
 3. 根据 `tag` 找到组中特定的块。如果块中的 valid 位有效则继续下一步，如果无效，则直接访问主存，进行一致性处理。
 4. 根据 `block offset` 找到块中对应的数据；
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/09/20240906194714.png)
