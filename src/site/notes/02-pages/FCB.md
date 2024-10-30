---
{"dg-publish":true,"permalink":"/02-pages/FCB/","tags":["personal/blog","os/file"]}
---

一个 FCB 就是一个文件目录项。

文件控制块的结构取决于我们要对目录进行哪些操作：
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/10/20241018210933.png)

但是这样的 FCB 的容量过大。当我们需要对文件目录进行操作的时候，需要把 FCB 放入内存中，如果容量过大，就会导致频繁的 IO 操作，从而降低效率。我们注意到，当我们对文件进行操作的时候，只需要用到文件名这个信息，从而有了[[02-pages/索引结点\|索引结点]]。