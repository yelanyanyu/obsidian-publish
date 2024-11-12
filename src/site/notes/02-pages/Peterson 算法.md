---
{"dg-publish":true,"permalink":"/02-pages/Peterson 算法/","tags":["personal/blog","os/process","os/thread"]}
---

>假如我们不借助硬件的帮助，只用软件实现可行吗？这就是 Peterson 算法的背景。但是，这种方法由于支持多线程硬件的普及和复杂性逐渐淹没在历史的潮流中。
## 算法描述
形象化的描述：
![002-并发控制基础_2.png](/img/user/99-Resource/media/002-%E5%B9%B6%E5%8F%91%E6%8E%A7%E5%88%B6%E5%9F%BA%E7%A1%80_2.png)
**以下是算法的 c 语言实现：**
```c
int flag[2];
int turn;

void init() {
	flag[0] = flag[1] = 0; // 1->thread wants to grab lock
	turn = 0; // whose turn? (thread 0 or 1?)
}

void lock() {
	flag[self] = 1; // self: thread ID of caller
	turn = 1 - self; // make it other thread’s turn
	while ((flag[1 - self] == 1) && (turn == 1 - self))
	; // spin-wait
}

void unlock() {
	flag[self] = 0; // simply undo your intent
}
```
**解读：**
 - `flag[0]` 和 `flag[1]` 分别表示两个线程是否想要进入临界区。如果 `flag[i]` 为1，那么表示线程 `i` 想要进入临界区。    
 - `turn` 表示哪个线程有权进入临界区。如果 `turn` 的值为 `i`，那么线程 `i` 有权进入临界区。
 - `init` 函数是初始化函数，它将 `flag[0]` 和 `flag[1]` 设置为0，表示初始时没有线程想要进入临界区，并且将 `turn` 设置为0，表示初始时线程0有权进入临界区。
 - `lock` 函数是线程用来请求进入临界区的函数。线程首先将自己的 `flag[self]` 设置为1，表示自己想要进入临界区，然后将 `turn` 设置为 `1 - self`，表示让对方有机会首先进入临界区。然后，如果对方也想进入临界区并且对方有权进入临界区，那么就进行自旋等待。
 - `unlock` 函数是线程用来释放临界区的函数。线程将自己的 `flag[self]` 设置为0，表示自己不再需要进入临界区。

## 淘汰的原因
1. 忙等待：不符合[[02-pages/让权等待\|让权等待]]。
2. 死锁：当两个或多个进程试图同时进入临界区时，可能会发生死锁。这种情况下，进程无法继续执行，导致系统停止响应。  
3. 公平性问题：Peterson算法**不保证公平性**，即不保证每个进程都能按照它们申请资源的顺序进入临界区。这可能导致某些进程长时间地无法访问临界区，而其他进程却可以频繁地访问。  
4. 只**适用于两个进程**：Peterson算法仅适用于两个进程之间的互斥访问临界区，而不适用于多个进程之间的同步。  
5. 假设过强：Peterson算法假设了硬件和操作系统的一些特性，如原子性操作和强大的内存模型。这些假设可能在某些系统中不成立，使得算法无法正确工作。

