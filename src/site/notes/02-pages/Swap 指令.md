---
{"dg-publish":true,"permalink":"/02-pages/Swap 指令/","tags":["personal/blog","os/process","os/thread"]}
---

```c
int xchg(int *table, int *new) {
	int temp;
	temp = *table;
	*table = *new;
	*new = temp;
	return *table;
}
```