---
{"dg-publish":true,"permalink":"/02-pages/DRAM-动态RAM/","tags":["personal/blog","计算机组成原理"]}
---

使用[[02-pages/栅极电容\|栅极电容]]作为存储元。主要应用于主存中。

由于栅极电容的特定，DRAM 芯片需要每隔一个周期进行刷新操作（2 ms），每次刷新一行存储单元：[[02-pages/DRAM 的刷新\|DRAM 的刷新]]。

为了减少地址线数量和芯片引脚，所有对于 DRAM，有[[02-pages/地址线复用技术\|地址线复用技术]]。

有许多中 DRAM：
 - [[SDRAM\|SDRAM]]；