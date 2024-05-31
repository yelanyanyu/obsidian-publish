---
{"dg-publish":true,"permalink":"/02-pages/B树的添加/","tags":["personal/blog","algorithm/data-structures/有序表/平衡树/B树","algorithm/data-structures/有序表/平衡树"]}
---

B 树的扩充过程与寻常的平衡二叉树不同。其他二叉树的扩充是从根节点出发，逐渐往下扩充。而 B 树则是从下往上进行扩充。

```ad-note
title: Def
插入操作是指插入一条记录，即（key, value）的键值对。如果B树中已存在需要插入的键值对，则用需要插入的value替换旧的value。若B树不存在这个key,则一定是在叶子结点中进行插入操作。
```

# 基本步骤
1. 根据要插入的key的值，找到叶子结点并插入。
2. 判断当前结点key的个数是否小于等于m-1，若满足则结束，否则进行第3步。
3. 以结点中间的key为中心分裂成左右两部分，然后将这个中间的key插入到父结点中，这个key的左子树指向分裂后的左半部分，这个key的右子支指向分裂后的右半部分，然后将当前结点指向父结点，继续进行第3步。

# 演示
![[2024-05-10 22-58-00.mp4]]


我们可以用以下网站进行自定义的动态演示：
[B-Tree Visualization (usfca.edu)](https://www.cs.usfca.edu/~galles/visualization/BTree.html)

我们需要特别注意的是：
1. B 树添加节点时分裂的过程。分裂的过程是将中间元素送往父节点，将该节点分裂成两部分的过程。
2. 当关键字个数为 m 时会发生分裂；