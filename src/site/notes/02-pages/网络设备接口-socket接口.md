---
{"dg-publish":true,"permalink":"/02-pages/网络设备接口-socket接口/","tags":["personal/blog","os"]}
---

主要有如下系统调用：
 - socket：创建一个[[02-pages/网络套接字\|网络套接字]]，需指明网络协议（TCP or UDP）；
 - bind：将套接字绑定到某个本地的端口；
 - connect：将套接字连接到远程地址；
 - read、write：从套接字读写数据；


传输过程如下图所示：
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/08/20240806221748.png)
