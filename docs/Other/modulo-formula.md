# 取模运算

对于任意实数x和y，x对y取模的计算公式如下：

$$
\begin{aligned}
& x \mod y = x - y \times floor(\frac{x}{y}), \quad y\neq 0 \\
& x \mod 0 = x
\end{aligned}
$$

- $floor(\frac{x}{y})$表示对x除以y的值向下取整。

x对y取模的结果都介于0和模之间：

$$
\begin{aligned}
0 \leq x \mod y < y, \quad y>0 \\
0 \geq x \mod y > y, \quad y<0
\end{aligned}
$$

#### 参考资料

- [模（Mod）运算的运算法则、公式（不包括同余关系）](https://zhuanlan.zhihu.com/p/60533528)