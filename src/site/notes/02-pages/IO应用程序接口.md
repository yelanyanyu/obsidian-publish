---
{"dg-publish":true,"permalink":"/02-pages/IO应用程序接口/","tags":["personal/blog","os"]}
---

IO 设备有很多不同的类型，用户层的应用程序无法用一个统一的系统调用接口来完成所有类型设备的 IO。例如，我们无法使用 read 系统调用同时完成字符设备（键盘、打印机等）、块设备（磁盘）、网络设备。故而需要针对这三种设备开发不同的程序接口：
- [[02-pages/字符设备接口\|字符设备接口]]。字符设备是不可寻址的，每次只读一个字符；
- [[02-pages/块设备接口\|块设备接口]]。可寻址，每次可以读写一个块；
- [[02-pages/网络设备接口-socket接口\|网络设备接口-socket接口]]。一个特定设备特定进程的数据如何给到另一个设备的特定进程；