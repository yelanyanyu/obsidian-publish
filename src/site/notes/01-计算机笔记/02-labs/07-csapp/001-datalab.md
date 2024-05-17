---
{"dg-publish":true,"permalink":"/01-计算机笔记/02-labs/07-csapp/001-datalab/","tags":["personal/blog","csapp"]}
---

# 总述

# 题目解析
## 01-xor
```ad-question
title: Question
~~~c
//1
/* 
 * bitXor - x^y using only ~ and & 
 *   Example: bitXor(4, 5) = 1
 *   Legal ops: ~ &
 *   Max ops: 14
 *   Rating: 1
 */
int bitXor(int x, int y) {
  return 2;
}
~~~
题意大致是，只用取反符号（`~`）和按位与（`&`）完成异或的操作（`x ^ y`）。
```




答案：
```c
int bitXor(int x, int y)
{
  return (~(x & y)) & (~(~x & ~y));
}
```

## 02-tmin
```ad-question
title: Question
~~~c
/* 
 * tmin - return minimum two's complement integer 
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 4
 *   Rating: 1
 */
int tmin(void) {

  return 2;

}
~~~
只用规定的字符返回最小的补码整数。
```




# 总结