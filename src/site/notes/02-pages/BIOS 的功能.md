---
{"dg-publish":true,"permalink":"/02-pages/BIOS 的功能/","tags":["personal/blog","计算机组成原理","computer","os"]}
---

- 自检以及初始化程序：
	- 加电自检（POST）；
		功能是检查计算机**硬件是否良好**，自检中如发现有错误，将按两种情况处理：对于严重故障（致命性故障）则停机，此时由于各种初始化操作还没完成，不能给出任何提示或信号；对于非严重故障则给出提示或声音报警信号，等待用户处理。
	- 初始化；
		创建[[02-pages/中断向量\|中断向量]]、设置寄存器、对一些外部设备进行初始化和检测等，其中很重要的一部分是 **BIOS 设置**，主要是对硬件设置的一些参数，当计算机启动时会读取这些参数，并和实际硬件设置进行比较，如果不符合，会影响系统的启动。
	- 引导程序；
		功能是引导 Dos 或其他[[01-MOCs/操作系统-MOC\|操作系统]]，此时会在硬盘读取引导记录，然后把计算机的控制权转交给引导记录，由引导记录把操作系统装入电脑，在电脑启动成功后，BIOS 的这部分任务就完成了。
- 程序服务处理；
	BIOS 直接与计算机的 I/O 设备（Input/Output，即输入/输出设备）打交道，通过特定的数据端口发出命令，传送或者接受各种外部设备的数据，实现软件程序对硬件的直接操作。
- 硬件中断处理；
	开机时 BIOS 会告诉 CPU 各硬件设备的中断号，当用户发出使用某个设备的指令后，CPU 就根据中断号使用相对应的硬件完成工作，再根据中断号跳回原来的工作。