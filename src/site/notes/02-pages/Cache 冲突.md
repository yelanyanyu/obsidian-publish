---
{"dg-publish":true,"permalink":"/02-pages/Cache 冲突/","tags":["personal/blog","计算机组成原理"]}
---

给定两个内存单元号，如果这两个内存单元都映射到了同一个 Cache line，那么就引起了 Cache 冲突。

```ad-note
title: 注意
内存单元号对应的是内存地址，而不是主存块号。故而，在判断主存单元是否可能引起 Cache 冲突，需要**先计算出主存块号**，然后根据**映射方式**计算是否产生冲突。
```
