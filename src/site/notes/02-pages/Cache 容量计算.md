---
{"dg-publish":true,"permalink":"/02-pages/Cache 容量计算/","tags":["personal/blog","计算机组成原理"]}
---

# 基本概念
[[02-pages/Cache 行\|Cache 行]]。

# 计算步骤
1. 先计算标记位；
2. 计算 Cache 块的位数。若主存块的位数为 N 位，则块内地址的位数为 $\displaystyle \log_{2}N$；
3. 计算 Cache 行 （line） 的位数 = 标记项位数 + Cache 块位数；
4. 计算总容量 = Cache 行数 * Cache 行（line） 的位数；

[[02-pages/写回法\|写回法]]