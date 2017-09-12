#### SMO

求对偶问题\(6\)的最佳解$$\alpha^*$$可以使用动态规划，然而在样本点很多的情况下复杂度会变得很高，于是有高人就研究出了一个更高效的算法Sequential Minimal Optimization \(SMO\)。

SMO的主要思路是将\(6\)式这个二次规划问题分成一系列的很小的二次规划问题。

SMO每次选取两个变量$$\alpha_i, \alpha_j$$，并固定其他参数，选择两个变量的原因是因为约束\(7\)式，选择一个变量，而固定其他$$m-1$$变量的话，那这个变量本身已经可以被计算出来了。

两个变量$$\alpha_i , \alpha_j$$上的优化问题可以有解析解，避免了数值解，从而计算效率大大提升。于是我们将SMO分成两个部分讨论：1）两个拉格朗日乘子变量上的优化问题的解析解；2）启发式地决定选择哪两个拉格朗日乘子。

#### 解析解

将这两个拉格朗日乘子记作$$\alpha_1, \alpha_2$$，分别表示每步迭代中的第一个和第二个乘子，

##### 取值范围

根据上一篇文章\(7\)式约束条件有，

$$\alpha_1 y_1 + \alpha_2 y_2 = \zeta, \quad \alpha_1 \ge 0, \alpha_1 \ge 0$$                                                                                   \(9\)

其中常数（在当前选择$$\alpha_1, \alpha_2$$ 这两个变量时，其他乘子都被固定，可以看作常数）为，

$$\zeta = - \sum_{k \neq 1,2} \alpha_k y_k$$

由于$$y_i \in \lbrace -1, 1 \rbrace, \forall i \in [m]$$，当$$y_1 = -y_2$$ 时，如下图，

![](/assets/SMO_constraint.png)

我们用$$\alpha2$$ 来表示$$\alpha_1$$，所以考察$$\alpha_2$$ 的范围，从上图中可以看的很清楚了，

1\) 当$$\zeta \ge 0 $$ 时，$$\alpha_2$$ 的上限$$H$$ 为$$C- \zeta \le C$$，$$\alpha_2$$ 的下限$$L$$ 恒为$$0 \ge -\zeta$$

2\) 当$$\zeta \le 0$$ 时，$$\alpha_2$$ 的上限$$H$$ 恒为$$C \le C - \zeta$$，$$\alpha_2$$ 的下限$$L$$ 为$$- \zeta \ge 0$$

综合以上两点得到$$\alpha_2$$ 的上下限，

$$L = max(0, -\zeta) \quad H = min(C,  C - \zeta)$$

或，

$$L = max(0, \alpha_2 - \alpha_1) \quad H = min(C,  C + \alpha_2 - \alpha_1)$$

同理，当$$y_1=y_2$$时，$$\alpha_2$$ 的上下限为，

$$L = max(0, \alpha_2 + \alpha_1 -C) \quad H = min(C,  \alpha_2 + \alpha_1)$$

##### 最佳解

将$$\alpha_1 , \alpha_2$$ 之外的看作常量，将上一篇文章中的\(6\)式目标函数改写为如下，

$$\Psi = \frac 1 2(\mathbf x_1^2 \alpha_1^2 + \mathbf x_2^2 \alpha_2^2 + 2s \cdot \mathbf x_1 \mathbf x_2 \alpha_1 \alpha_2) + v_1 y_1 \alpha_1 + v_2 y_2 \alpha_2 - \alpha_1 - \alpha_2 + \Psi_{const}$$               \(10\)

$$s = y_1y_2$$

$$v_i = \sum_{j=3}^m y_j \alpha_j \mathbf x_j \mathbf x_i = f(\mathbf x_i) - b^{(p)} - y_1 \mathbf x_1 \mathbf x_i \alpha_1^{(p)} - y_2 \mathbf x_2 \mathbf x_i \alpha_2^{(p)}, \quad i = 1,2$$

上式中，带$$(p)$$ 上标的表示上一次迭代得到的值（在本次迭代中也看作常数），由于

$$y_1 \alpha_1^{(p)} + y_2 \alpha_2^{(p)} = \zeta = y_1 \alpha_1 +y_2 \alpha_2$$

$$\zeta$$ 的值在本次迭代前后不变，上式两边同时乘以$$y_1$$，

$$\alpha_1 + s \alpha_2 = \alpha_1^{(p)} + s \alpha_2^{(p)} = w \Rightarrow \alpha_1 = w - s \alpha_2$$

其中$$w = y_1 \zeta$$。

代入\(10\)式得到关于$$\alpha_2$$ 的二次函数，

$$\Psi = \frac 1 2 K_{11} (w - s \alpha_2)^2 + \frac 1 2 K_{22} \alpha_2^2 + s K_{12}(w - s \alpha_2) \alpha_2 + v_1 y_1(w -s \alpha_2) + v_2 y_2 \alpha_2 - (w - s \alpha_2) - \alpha_2 + \Psi_{const}$$

$$K_{ij} = \mathbf x_i \mathbf x_j$$

求导，令导数等于0求得极值解，

$$\frac {d \Psi} {d \alpha2} = -s K_{11} (w - s \alpha_2) + K_{22} \alpha_2 + s K_{12} (w - s \alpha_2) - K_{12} \alpha_2 - v_1 y_2 + v_2 y_2 + s -1 = 0$$

对上式化简和移项得，

$$(K_{11} + K_{22}  - 2K_{12}) \alpha_2 = sw(K_{11} - K_{12}) + y_2(v_1 - v_2) + 1 - s$$                                            \(11\)

综合$$w, s, v_1, v_2, \zeta$$  表达式以及上式，我们只看右边项，

$$\begin{align} & sw (K_{11} - K_{12}) - y_2(v_1 -v_2) + 1 - s \\ & = sw (K_{11} - K_{12}) + y_2[f_1 - b^{(p)}  - y_1 K_{11} \alpha_1^{(p)} - y_2 K_{12} \alpha_2^{(p)} - f_2 + b^{(p)} + y_1 K_{12} \alpha_1^{(p)} + y_2 K_{22} \alpha_2^{(p)}] + 1 - s \\ & = sw(K_{11}-K_{12}) + y_2[f_1 - f_2 - y_1(K_{11} - K_{12}) \alpha_1^{(p)} - y_2(K_{12} - K_{22}) \alpha_2^{(p)}] + 1 - s \\ & = sw(K_{11} - K_{12}) + y_2(f_1- f_2) - y_1y_2(K_{11} - K_{12}) \alpha_1^{(p)} - y_2^2(K_{12} - K_{22})\alpha_2^{(p)} + 1 - s \\ & = sw(K_{11} - K_{12}) + y_2(f_1 -f_2) - s(K_{11} - K_{12})(w - s \alpha_2^{(p)}) - (K_{12} - K_{22}) \alpha_2^{(p)} + 1 -s \\ & = y_2(f_1 - f_2)  + (K_{11} - K_{12}) \alpha_2^{(p)} - (K_{12} - K_{22}) \alpha_2^{(p)} + 1 - s \\ & = y_2(f_1 -f_2) (K_{11} + K_{22} - 2K_{12}) \alpha_2^{(p)} + y_2^2 - y_2y_1 \\ & = (K_{11} + K_{22} - 2 K_{12}) \alpha_2^{(p)} + y_2[f_1 - y_1 - (f_2 - y_2)] \end{align}$$

其中，$$f_i = f(\mathbf x_i)$$

记

$$\eta = (K_{11} + K_{22} - 2 K_{12})$$

代入\(11\)式得到，

$$\alpha_2^{new} = \alpha_2^{(p)} + \frac {y_2} \eta (E_1- E_2)$$                                                                                                               \(12\)

其中，$$E_i = f_i-y_i$$ 表示第$$i$$ 个样本点的迭代前误差。

##### 修剪

\(12\)式就是迭代后$$\alpha_2$$的新值，然而这个新值不一定满足上面分析的$$\alpha_2$$ 的上下限，所以我们还需要对这个迭代后计算的$$\alpha_2$$ 值进行修剪如下，

$$ \alpha_2^{new, clipped} =   \begin{cases} H \quad & if \quad \alpha_2^{new} \ge H \\ \alpha_2^{new} \quad & if \quad L \lt \alpha_2^{new} \lt H \\ L \quad & if \quad \alpha_2^{new} \le L \end{cases}$$

于是，

$$\alpha_1^{new} = w - s \alpha_2^{new} = \alpha_1^{(p)} + s \alpha_2^{(p)} - s \alpha_2^{new} = \alpha_1^{(p)} + s(\alpha_2^{(p)} - \alpha_2^{new, clipped}) $$                             \(13\)

下面我们分析为何可以通过对目标函数求导就可以求得最佳解，目标函数的二次导为，

$$\Psi'' = \frac {\Psi'} {d \alpha_2} = K_{11} + K_{22} - 2 K_{12} = \eta$$

1. 当$$\eta > 0 $$ 时，$$\Psi$$ 是下凸函数，最小值在$$arg_{\alpha_2} \ \Psi '$$ 处
2. 当$$\eta = 0$$ 时，$$\Psi$$ 是单调函数，最小值在左端点（单调增）或右端点（单调减）处
3. 当$$\eta < 0$$ 时，$$\Psi$$ 是上凹函数，最小值在左端点或右端点处（比较一下就知道究竟是哪个端点）

##### 特殊情况

所以实际计算时，如果碰到$$\eta \le 0$$ 的情况，需要计算两个端点处的$$\Psi$$值，然后比较得出最小值对应的是$$\alpha_2 = L$$ 还是$$\alpha_2 =H$$，计算过程如下，

对迭代后的$$\alpha_2$$，

根据\(13\)式，迭代后的

$$\alpha_1 = \alpha_1^{(p)} + s(\alpha_2^{(p)} - \alpha_2) $$

因为我们要计算$$\Psi$$，根据\(10\)式，除了$$\alpha_1, \alpha_2$$，其他都看作常量，所以$$v_i$$ 中的$$f_i$$ 使用迭代前的值，如下

$$ f_1 = \sum_{j=1}^m y_j \alpha_j K_{1j} + b = \sum_{j=3}^m y_j \alpha_j K_{1j} + y_1\alpha_1^{(p)} K_{11} + y_2 \alpha_2^{(p)} K_{12} + b$$

于是（忽略常数项，因为不影响$$\Psi|_{\alpha_2 = L} \ ? \ \Psi| _{\alpha_2 = H}$$ 的大小关系），

$$\Psi = \frac 1 2 (K_{11} \alpha_1^2 + K_{22} \alpha_2^2 + 2s K_{12} \alpha_1 \alpha_2) + (v_1 y_1 - 1) \alpha_1 + (v_2 y_2 - 1) \alpha_2 $$

其中，

$$g_1 = v_1y_1 - 1 = y_1(f_1 - b - y_1 K_{11} \alpha_1^{(p)} - y_2K_{12} \alpha_2^{(p)}) - y_1^2 =y_1(f_1 - y_1 - b) - K_{11} \alpha_1^{(p)} - s K_{12} \alpha_2^{(p)} $$

$$g_2 = v_2y_2 - 1 = y_2(f_2 - b - y_1 K_{12} \alpha_1^{(p)} - y_2K_{22} \alpha_2^{(p)}) - y_2^2 =y_2(f_2 - y_2 - b) - s K_{12} \alpha_1^{(p)} -  K_{22} \alpha_2^{(p)}$$

综合上式，

$$\begin{cases} \Psi = \frac 1 2 K_{11}  \alpha_1^2 + \frac 1 2 K_{22} \alpha_2 + s K_{12} \alpha_1 \alpha_2 + g_1 \alpha_1 + g_2 \alpha_2 \\ \alpha_1 = \alpha_1^{(p)} + s(\alpha_2^{(p)} - \alpha_2) \\ g_1 =y_1(f_1 - y_1 - b) - K_{11} \alpha_1^{(p)} - s K_{12} \alpha_2^{(p)} \\ g_2 =y_2(f_2 - y_2 - b) - s K_{12} \alpha_1^{(p)} -  K_{22} \alpha_2^{(p)} \end{cases}$$

于是分别将$$\alpha_2 = L$$ 和$$\alpha_2 =H$$ 代入上面方程组求的$$\Psi$$ 较小值确定$$\alpha_2$$应该取哪个端点。

