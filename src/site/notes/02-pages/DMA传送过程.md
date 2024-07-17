---
{"dg-publish":true,"permalink":"/02-pages/DMA传送过程/","tags":["personal/blog","计算机组成原理"]}
---

- 预处理；
- 数据传送；
	1. 设备先向数据缓冲寄存器写入数据，当写完一个字后，就通过 DMA 请求触发器发送高电平信号；
	2. 然后控制、状态逻辑检测到触发器变化后，就向 CPU 申请总线使用权；
	3. 通过数据线、地址线传输数据；
	4. 如果数据一整块传输完成（传送长度计数器越界），就通过中断机构发出中断，结束传输；
- 后处理；

![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/07/20240715105358.png)![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/07/20240715105408.png)