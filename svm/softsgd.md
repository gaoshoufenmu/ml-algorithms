在soft文章中我们给出目标优化问题为，

$$min_{\mathbf w, b, \mathbf \xi} \ \frac 1 2 |\mathbf w|^2 + C \sum_{i=1}^m \xi_i, \quad \forall i \in [m], \ y_i(\mathbf {wx}_i + b) \ge 1 - \xi_i, \ \xi_i \ge 0$$

这是由于放宽了约束条件$$y_i(\mathbf {wx}_i + b) \ge 1$$，引入松弛因子$$\xi$$，然而除了这种方法，其实还有很多其他方法，比如我们引入损失函数$$\mathcal l (z)$$，从而将优化问题转为，

$$min_{\mathbf w, b} \ \frac 1 2 |\mathbf w|^2 + C \sum_{i=1}^m \mathcal l (z)$$

使得$$|\mathbf w|$$ 减小的时候，两条边界间距增大，更多的点会越过对应边界（即，$$y_i(\mathbf {wx}_i + b) \lt 1$$），使得损失函数增大，从而第一项和第二项相互制约。

常见的损失函数有，

* 0-1损失函数

$$\mathcal l_{0/1} (z) = \begin{cases} 1, \quad z \lt 0 \\ 0, \quad otherwise \end{cases}$$

此时目标函数为

$$min_{\mathbf w, b} \ \frac 1 2 |\mathbf w|^2 + C \sum_{i=1}^m \mathcal l [y_i(\mathbf {wx}_i + b) - 1]$$

* hinge损失函数

$$\mathcal l_{hingle}(z) = max(0, 1 - z)$$

目标函数为，

$$min_{\mathbf w, b} \ \frac 1 2 |\mathbf w|^2 + C \sum_{i=1}^m max(0, 1- y_i(\mathbf {wx}_i + b))$$

* 指数损失函数

$$\mathcal l_{exp}(z) = exp(-z)$$

* 对率损失函数

$$\mathcal l_{log} (z) = log(1+exp(-z))$$

#### SGD求解

以hingle损失函数为例，此时Soft-SVM优化问题为，

$$min_{\mathbf w} \ \Psi(\mathbf w)$$                                                                                                            \(1\)

其中，$$\Psi (\mathbf w) = \frac 1 2 |\mathbf w|^2 + C \sum_{i=1}^m max(0, 1- y_i(\mathbf {wx}_i + b))$$

根据随机梯度下降章节中的强凸函数Strongly Convex文章的介绍，这里$$\Psi(\mathbf w)$$ 是一个参数为1 的强凸函数，于是根据强凸函数的随机梯度下降法步骤，要求第$$t$$ 步的$$\Psi(\mathbf w)$$ 的次梯度，而$$\Psi$$ 的第一项导数为$$\mathbf w$$，第二项是损失函数，

$$l(\mathbf w, i) = C \cdot max(0, 1 - y_i(\mathbf {wx}_i + b)) $$

等概率选取样本点$$i$$，

$$m \cdot \Bbb E [\mathbf v_t | \mathbf w^{(t)}] = m \cdot \Bbb E_{i \sim [m]}[\nabla l (\mathbf w^{(t)}, i)] =\nabla ( m \cdot \Bbb E_{i \sim [m]}[ l (\mathbf w^{(t)}, i)]) = \nabla ( \sum_{i=1}^m l (\mathbf w^{(t)}, i))$$

根据迭代公式更新$$\mathbf w: \ \mathbf w^{(t+1)} = \mathbf w^{(t)} - \eta_t \mathbf v_t'$$，于是，

$$\begin{aligned} \mathbf w^{(t+1)} & = \mathbf w^{(t)} - \frac 1 t (\mathbf w^{(t)} + m \mathbf v_t) \\ & = (1 - \frac 1 t) \mathbf w^{(t)} - \frac m t \mathbf v_t \\ & = \frac {t-1} t \mathbf w^{(t)} - \frac m t \mathbf v_t \\ & = \frac {t -1} t (\frac {t - 2} {t - 1} \mathbf w^{(t-1)} - \frac m {t-1} \mathbf v_{t-1}) - \frac m t \mathbf v_t \\ & = -\frac m t \sum_{i=1}^t \mathbf v_i \end{aligned}$$

而根据$$l(\mathbf w, i)$$可知，

$$\mathbf v_t = \begin{cases} 0, \quad & y(\mathbf {w^{(t)}x} + b) < 1 \\ -yC \mathbf x, \quad & otherwise \end{cases}$$

为了方便，令$$\theta^{(t)} = - \sum_{j \lt t} \mathbf v_j$$

总结SGD求解Soft-SVM步骤如下，

目标：求解\(1\)

参数：迭代次数$$T$$

初始化：$$\theta^{(1)} = \mathbf 0$$

循环：$$for \ t = 1, ..., T$$

1. 令$$\mathbf w^{(t)} = \frac m t \theta^{(1)}$$
2. 从集合$$[m]$$ 中随机选择样本点下标$$i$$，如果$$y_i(\mathbf {w^{(t)}x_i} + b) < 1$$，那么，$$\theta^{(t+1)} = \theta^{(t)} + y_i C \mathbf x_i$$，否则，$$\theta^{(t+1)} = \theta^{(t)}$$

输出：$$\overline {\mathbf w} = \frac 1 T \sum_{i=1}^T \mathbf w^{(t)}$$



