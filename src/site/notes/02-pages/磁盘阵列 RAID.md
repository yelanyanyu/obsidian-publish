---
{"dg-publish":true,"permalink":"/02-pages/磁盘阵列 RAID/","tags":["personal/blog","计算机组成原理"]}
---

将多个独立的物理磁盘组成一个独立的逻辑盘，数据在多个物理磁盘上分割交叉存储、并行访问，具有更好的存储性能、可靠性和安全性。

![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/08/20240822202323.png)

![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/08/20240822202338.png)

```ad-note
title: RAID0
RAID0把连续多个数据块交替地存放在不同物理磁盘的扇区中，几个磁盘交叉并行读写，不仅扩大了存储容量，而且提高了磁盘数据存取速度，但RAID0没有容错能力。
```

```ad-note
title: RAID1
RAID1是为了提高可靠性，使两个磁盘同时进行读写，互为备份，如果一个磁盘出现故障，可从另一磁盘中读出数据。两个磁盘当一个磁盘使用，意味着容量减少一半。
```

