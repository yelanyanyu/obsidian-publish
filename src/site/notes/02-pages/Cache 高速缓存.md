---
{"dg-publish":true,"permalink":"/02-pages/Cache 高速缓存/","tags":["personal/blog","计算机组成原理"]}
---

由[[02-pages/存储器的层次结构\|存储器的层次结构]]可知，Cache 是一种仅次于 CPU 的高速存储器，主要由 [[02-pages/SRAM-静态RAM\|SRAM-静态RAM]] 组成。

Cache 的构想来自于[[02-pages/局部性原理\|局部性原理]]。把 CPU 目前访问的地址周围的部分数据放入到 Cache 中。这样就缓和了 CPU 的速度和主存速度不匹配的问题，提高了 [[02-pages/Cache 性能\|Cache 性能]]。

但是主存块过大同样也会破坏局部性原理：[[02-pages/Cache 命中率规律\|Cache 命中率规律]]。

要完成一个功能完善的 Cache，需要解决如下三个问题：
- 当主存块的内容复制到 Cache 中，如何选择存放在 Cache 中的位置？[[02-pages/Cache 和 主存的映射方式\|Cache 和 主存的映射方式]]。
- 如果 Cache 满了，怎么办？[[02-pages/Cache 替换算法\|Cache 替换算法]]。
- 如果 CPU 修改了 Cache 中的数据，如何同步修改主存的内容。[[02-pages/Cache 的写策略\|Cache 的写策略]]。
- 如何通过一个逻辑地址找到对应主存的位置呢？[[02-pages/Cache 寻址过程\|Cache 寻址过程]]。

[[02-pages/Cache 容量计算\|Cache 容量计算]]。

```ad-tip
title: 注意
Cache 上述的功能**全部由硬件实现**。
```
