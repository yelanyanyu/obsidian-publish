---
{"dg-publish":true,"permalink":"/02-pages/最坏适应算法（Worst Fit）/","tags":["personal/blog"]}
---

又称最大适应算法（Largest Fit）

算法思想：为了解决最佳适应算法的问题一一即留下太多难以利用的小碎片，可以在每次分配时优先使用最大的连续空闲区，这样分配后剩余的空闲区就不会太小，更方便使用。

如何实现：与[[02-pages/最佳适应算法\|最佳适应算法]]截然相反，空闲分区按容量递减次序链接。每次分配内存时顺序查找空闲分区链（或空闲分区表），找到大小能满足要求的第一个空闲分区。

缺点：每次都选最大的分区进行分配，虽然可以让分配后留下的空闲区更大，更可用，但是这种方式会导致较大的连续空闲区被迅速用完。如果之后有“大进程”到达，就没有内存分区可用了。