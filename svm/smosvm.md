#### SMO

求对偶问题\(6\)的最佳解$$\alpha^*$$可以使用动态规划，然而在样本点很多的情况下复杂度会变得很高，于是有高人就研究出了一个更高效的算法Sequential Minimal Optimization \(SMO\)。

SMO的主要思路是将\(6\)式这个二次规划问题分成一系列的很小的二次规划问题。

SMO每次选取两个变量$$\alpha_i, \alpha_j$$，并固定其他参数，选择两个变量的原因是因为约束\(7\)式，选择一个变量，而固定其他$$m-1$$变量的话，那这个变量本身已经可以被计算出来了。~~好了，选择了两个变量~~$$\alpha_i , \alpha_j$$~~后，求解\(6\)式的最优解，~~

两个变量$$\alpha_i , \alpha_j$$上的优化问题可以有解析解，避免了数值解，从而计算效率大大提升。于是我们将SMO分成两个部分讨论：1）两个拉格朗日乘子变量上的优化问题的解析解；2）启发式地决定选择哪两个拉格朗日乘子。

#### 解析解

将这两个拉格朗日乘子记作$$\alpha_1, \alpha_2$$，分别表示每步迭代中的第一个和第二个乘子，

根据上一篇文章\(7\)式约束条件有，

$$\alpha_1 y_1 + \alpha_2 y_2 = \zeta, \quad \alpha_1 \ge 0, \alpha_1 \ge 0$$                                                                                   \(9\)

其中常数（在当前选择$$\alpha_1, \alpha_2$$ 这两个变量时，其他乘子都被固定，可以看作常数）为，

$$\zeta = - \sum_{k \neq 1,2} \alpha_k y_k$$

由于$$y_i \in \lbrace -1, 1 \rbrace, \forall i \in [m]$$，有如下图，



根据\(5\)式得到

$$\alpha_j = \frac {c - \alpha_i y_i} {y_j}$$                                                                                                                             \(6\)

代入上一篇文章中的优化问题\(4\)式消去$$\alpha_j$$，得到只包含$$\alpha_i$$ 一个变量的二次规划问题，此外还有约束$$\alpha_i \ge 0$$，于是，求其最佳解通过对$$\alpha_i$$ 求导并令其等于0，

先看\(4\)中的两重求和项，

$$(\alpha_i y_i \mathbf x_i + \alpha_j y_j \mathbf x_j + C)^2 = (\alpha_i y_i \mathbf x_i)^2 + (\alpha_j y_j \mathbf x_j)^2 + C^2 + 2(\alpha_i y_i \mathbf x_i + \alpha_j y_j \mathbf x_j)C + 2(\alpha_i y_i \mathbf x_i  \cdot \alpha_j y_j \mathbf x_j)$$

其中常数项C为，

$$C = \sum_{k \neq i, j} \alpha_k y_k \mathbf x_k$$                                                                                                                \(7\)

于是\(4\)式对$$\alpha_i$$ 求导等于0解得最佳解，

$$\frac {\partial f(\alpha_i)} {\alpha_i} = -\frac 1 2 [2y_i^2 \mathbf x_i^2 \alpha_i + 2y_i \mathbf x_j^2 (y_i \alpha_i - c) + 2 C y_i(\mathbf x_i - \mathbf x_j) + 2 \mathbf x_i \mathbf x_j y_i c - 4 \mathbf x_i \mathbf x_j y_i^2 \alpha_i] + 1 - \frac {y_i} {y_j} = 0$$

注意到$$y_i^2 = 1$$，所以解得，

$$\alpha_i = [1 - y_i / y_j  +  c y_i \mathbf x_j^2 -  C y_i (\mathbf x_i -  \mathbf x_j) -  c y_i \mathbf x_i \mathbf x_j] / (\mathbf x_i -  \mathbf x_j)^2$$

总结，解析解为，

$$\begin{cases}  C = \sum_{k \neq i, j} \alpha_k y_k \mathbf x_k \\ c = - \sum_{k \neq i,j} \alpha_k y_k \\ \alpha_i = [1 - y_i / y_j  +  c y_i \mathbf x_j^2 -  C y_i (\mathbf x_i -  \mathbf x_j) -  c y_i \mathbf x_i \mathbf x_j] / (\mathbf x_i -  \mathbf x_j)^2 \\ \alpha_j = (c - \alpha_i y_i) / y_j \end{cases}$$

注意，上面这个解析解有可能推导出错，毕竟没有仔细检查。

启发式选择变量

每次迭代需要选择两个变量$$\alpha_i, \alpha_j$$，分别对应优化问题的外层循环和内层循环，

