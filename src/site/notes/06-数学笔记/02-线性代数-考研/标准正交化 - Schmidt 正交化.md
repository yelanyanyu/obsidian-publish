---
{"dg-publish":true,"permalink":"/06/02/schmidt/","tags":["personal/blog","线性代数/向量"]}
---

# 定义
若向量组 $\displaystyle \alpha_{1},\alpha_{2},\alpha_{3}$ 线性无关，其标准正交化的方法如下：

## 正交化
取：
$$
\begin{align}
&\beta_{1}=\alpha_{1}, \\
&\beta_{2}=\alpha_{2}-\frac{(\alpha_{2},\beta_{1})}{(\beta_{1},\beta_{1})}\beta_{1} \\
&\beta_{3}=\alpha_{3}-\frac{(\alpha_{3},\beta_{1})}{(\beta_{1},\beta_{1})}\beta_{1}-\frac{(\alpha_{3},\beta_{2})}{(\beta_{2},\beta_{2})}\beta_{2}
\end{align}
$$
求得的 $\displaystyle \beta_{1},\beta_{2},\beta_{3}$ 是正交向量组。

假设向量 $\alpha_{3} = (a_1, a_2, a_3)$，向量 $\beta_{1} = (b_1, b_2, b_3)$，其中 $a_1, a_2, a_3, b_1, b_2, b_3$ 是向量的坐标分量, 则：
$\displaystyle (\alpha_{3},\beta_{1}) = a_1b_1 + a_2b_2 + a_3b_3$


## 单位化
取：
$$
\eta_{1}=\frac{\beta_{1}}{|\beta_{1}|},\eta_{2}=\frac{\beta_{2}}{|\beta_{2}|},\eta_{3}=\frac{\beta_{3}}{|\beta_{3}|}
$$
求出的 $\displaystyle \eta_{1},\eta_{2},\eta_{3}$ 即为标准正交向量组。