线性预测包括：半空间（二值分类），线性回归，logistic回归。

相关算法有：二值分类用到的线性规划和感知机（Perceptron）算法，线性回归的最小二乘法。

定义仿射函数

$$L_d = \lbrace \langle \mathbf w, \mathbf x \rangle + b : \mathbf w \in \Bbb R ^ d, b \in \Bbb R \rbrace$$                                                             \(1\)

不同假设空间的线性预测机输出则是应用函数$$\phi : \Bbb R \rightarrow \mathcal Y$$ 到$$L_d$$ 上。比如，对于二值分类，选择$$\phi = sign$$ 阶跃函数，对于回归问题，选择$$\phi = I$$ （Identity Function）。

可以将偏置$$b$$ 看作是权重$$\mathbf w_0 = b$$ 对应输入分量$$\mathbf x_0 = 1$$，则，

$$L_d = \lbrace \langle \mathbf w, \mathbf x \rangle : \mathbf w \in \Bbb R ^ {d+1} \rbrace$$                                                                              \(2\)

$$\mathbf w = (b, w_1, ..., w_d)$$

$$\mathbf x = (1, x_1, ...,x_d)$$

假设线性可分的训练集为$$S = \lbrace (\mathbf x_i, y_i) \rbrace _{i=1}^m $$，ERM 预测机对训练集的误差点为0，即，

$$sign(\langle \mathbf w, \mathbf x_i \rangle) = y_i, \quad \forall i = 1,...,m$$                                                            \(3\)

等价地有，

$$y_i \langle \mathbf w, \mathbf x_i \rangle \gt 0, \quad \forall i = 1,...,m$$                                                                       \(4\)

假设$$\mathbf w^*$$ 满足\(4\)式（因为假设了线性可分，所以$$\mathbf w^*$$ 一定存在），定义

$$\gamma = min_{i} \ (y_i \langle \mathbf w^*, \mathbf x_i \rangle), \quad \overline {\mathbf w} = \frac {\mathbf w^*} \gamma$$

于是，

$$y_i \langle \overline {\mathbf w}, \mathbf x_i \rangle = \frac 1 \gamma y_i \langle \mathbf w^*, \mathbf x_i \rangle \ge 1, \quad \forall i = 1,...,m$$                                         \(5\)

感知机

感知机是一种迭代算法，假设在$$t$$ 次迭代，下标为$$i$$ 的样本点判断有误，

$$y_i \langle \mathbf w^{(t)}, \mathbf x_i \rangle \le 0$$                                                                                                   \(6\)

作以下更新，

$$\mathbf w^{(t+1)} = \mathbf w^{(t)} + \eta y_i \mathbf x_i$$                                                                                       \(7\)

其中，$$\eta \in (0, 1]$$ 表示步长，于是，

$$y_i \langle \mathbf w^{(t+1)}, \mathbf x_i \rangle = y_i \langle \mathbf w^{(t)} + \eta y_i \mathbf x_i, \mathbf x_i \rangle = y_i \langle \mathbf w^{(t)}, \mathbf x_i \rangle + \eta |\mathbf x_i|^2 \gt y_i \langle \mathbf w^{(t)}, \mathbf x_i \rangle$$

相比\(6\)式，更接近正确（大于0时就表示分类正确），这说明更新是有效的，正在朝着分类正确的方向更新。

算法：

输入：训练集$$\lbrace (\mathbf x_1,y_1), ..., (\mathbf x_m, y_m) \rbrace$$

初始化：$$\mathbf w^{(1)} = (0,...,0), \quad \eta = 1$$

过程：

$$for \ t = 1, 2,...$$

$$ \quad if \ (\exists i, y_i \langle \mathbf w^{(t)}, \mathbf x_i \rangle \le 0 ) \ then $$

$$\quad \quad \mathbf w^{(t+1)} = \mathbf w^{(t)} + \eta y_i \mathbf x_i$$

$$\quad else$$

$$\quad \quad output \ \mathbf w^{(t)}$$

