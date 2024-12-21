---
{"dg-publish":true,"permalink":"/02-pages/具有 TLB 和 Cache 的多级存储系统的访存/","tags":["personal/blog","os","计算机组成原理"]}
---

这里我们以[[02-pages/请求分页存储管理\|请求分页存储管理]]方式来说明 [[02-pages/快表\|TLB]] 和 [[02-pages/Cache 高速缓存\|Cache]] 相互配合的逻辑。一个很大的麻烦是 TLB 和 Cache 都是为了解决 CPU 和主存速度不一致的问题而提出的，故而可能会有人以为这两个硬件是同一个硬件，或者是互相包含的关系。但是事实不是这样的：
 - TLB 专注的是虚拟地址转化为物理地址这个过程。也就是在 [[02-pages/基本地址变换机构\|MMU]] 中的操作。主要与内存中的[[02-pages/页表\|页表]]进行交互。
 - Cache 主要关注经过虚拟地址转化后，优化从内存根据物理地址（存放在 [[MAR\|MAR]] 中）中取数据的过程。

一图胜千言：
![](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/12/%E8%AE%A1%E7%BB%84%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%E5%AD%98%E5%82%A8%E7%B3%BB%E7%BB%9F%E4%B8%B2%E8%AE%B2_1.png)

```ad-note
title: 解读
1. 需要注意的是页表位于 PCB 中，是独属于进程的，页表中存储的是虚拟地址到物理页框的映射，我们可以通过访问页表形成物理地址。
```

