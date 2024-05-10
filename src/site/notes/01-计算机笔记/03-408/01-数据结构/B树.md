---
{"dg-publish":true,"permalink":"/01/03-408/01/b/","tags":["personal/blog","algorithm/数据结构/平衡树"]}
---

# 为什么要有 B 树？
主要是为了解决磁盘 IO 慢的问题。传统的二叉平衡树，例如平衡树、BST，只有度数最多只有 2，那么就不可避免的使得树的深度过大。树的深度过大就会导致查找一个关键字的次数变多。所以，为了缓和这个矛盾，只能增加树的度数，于是 B 树就应运而生了。
# B 树的结构
[[01-计算机笔记/03-408/01-数据结构/B树的结构\|B树的结构]]
# B 树的操作
[[01-计算机笔记/03-408/01-数据结构/B树的操作\|B树的操作]]

# B 树的瓶颈
[[01-计算机笔记/03-408/01-数据结构/B+树\|B+树]]

# 参考资料
[B树和B+树的插入、删除图文详解 - nullzx - 博客园 (cnblogs.com)](https://www.cnblogs.com/nullzx/p/8729425.html)

[终于把B树搞明白了(一)_B树的引入，为什么会有B树_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1mY4y1W7pS/?spm_id_from=333.337.search-card.all.click&vd_source=71ed91ed82694d3cc5376be556d8c499)