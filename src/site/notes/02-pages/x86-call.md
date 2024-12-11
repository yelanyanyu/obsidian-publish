---
{"dg-publish":true,"permalink":"/02-pages/x86-call/","tags":["personal/blog","汇编语言","计算机组成原理/指令系统"]}
---

格式：`call <函数名>`。
作用：
 - 将 [[02-pages/程序计数器\|IP]] 旧值压栈保存（保存在[[02-pages/栈帧\|栈帧]]顶部）。
 - 设置 [[02-pages/程序计数器\|IP]] 新值，无条件转移至被调用函数的第一条指令。