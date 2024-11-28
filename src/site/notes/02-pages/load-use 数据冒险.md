---
{"dg-publish":true,"permalink":"/02-pages/load-use 数据冒险/","tags":["personal/blog","计算机组成原理/CPU"]}
---

如果 load 指令与其后紧邻的运算类指令存在数据相关问题, 则无法通过转发技术来解决。对于 load-use 数据冒险, 最简单的做法是由编译器在 add 指令之前插入一条 nop 指令。

```
load r2, 12(r1) # M[(r1)+12]->(r2)
add r4,r3,r2    # (r3)+(r2)->(r4)
```
例如上述指令就会出现该错误。add 指令进入 EX 阶段的前提是 load 指令的访存阶段（MEM）阶段结束。
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/11/20241128220514.png)

我们可以直接延迟一个机器周期，然后将 load 指令的 MEM 的结果直接传输给 add 指令。

最好的办法是预防，即在编译的时候由编译程序来尽量避免这种指令序列的出现。