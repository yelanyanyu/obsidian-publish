---
{"dg-publish":true,"permalink":"/02-pages/TestAndSet/","tags":["personal/blog","os/process","os/thread"]}
---

```c
int xchg(int *table, int new) {
    int old = *table; 
    *table = new;
    return old;
}
```