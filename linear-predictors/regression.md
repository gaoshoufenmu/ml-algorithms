根据上一篇文章的介绍，已经知道线性回归使用如下函数表示，

$$\mathcal H = L_d = \lbrace \langle \mathbf w, \mathbf x \rangle: \mathbf w \in \Bbb R^d, b \in \Bbb R \rbrace$$                                                                           \(1\)

定义损失函数为，

$$l(\mathbf x, y) = (\langle \mathbf w, \mathbf x \rangle - y)^2$$                                                                                                       \(2\)

均方误差为，

$$L_S = \frac 1 m \sum_{i=1}^m (\langle \mathbf w, \mathbf x_i \rangle  - y_i)^2$$                                                                                           \(3\)

#### 最小二乘法

我们的目标是求解，

$$argmin_{\mathbf w} \ L_S = argmin_{\mathbf w} \ \frac 1 m \sum_{i=1}^m (\langle \mathbf w, \mathbf x_i \rangle  - y_i)^2$$                                                    \(4\)

计算上式目标函数的梯度，并令其为0，

$$\nabla_{\mathbf w} \ L_S = \frac 2 m \sum_{i=1}^m (\langle \mathbf w, \mathbf x_i \rangle - y_i) \mathbf x_i = \mathbf 0$$                                                                       \(5\)

其实\(5\)式是一个由$$d$$ 个方程组成的方程组，将\(5\)式写成如下形式，

$$\mathbf {Aw} = \mathbf b$$                                                                                                                                 \(6\)

$$\mathbf A_{d \times d} = (\sum_{i=1}^m \mathbf x_i \mathbf x_i^T)$$

$$\mathbf b_{d \times 1} = \sum_{i=1}^m y_i \mathbf x_i$$

$$\mathbf w: d \times 1, \ \mathbf x : d \times 1$$

\(1\) 矩阵 $$\mathbf A$$ 可逆，那么

$$\mathbf w = \mathbf A^{-1} \mathbf b$$                                                                                                                             \(7\)

\(2\) 矩阵$$\mathbf A$$ 不可逆，使用梯度下降法，更新如下，

$$\mathbf w^{(t+1)} = \mathbf w^{(t)} - \eta \cdot \nabla_{\mathbf w} L_S|_{\mathbf w^{(t)}} = \mathbf w^{(t)} - \frac {2 \eta} m [ \sum_{i=1}^m (\langle \mathbf w^{(t)}, \mathbf x_i \rangle - y_i) \mathbf x_i ]$$                \(8\)      

#### 多项式回归

假设输入为一维，使用度为$$n$$ 的一维多项式函数描述如下，

$$p(x) = a_0 + a_1 x + a_2+x^2 + \cdot \cdot \cdot + a_nx^n$$

定义

$$\mathbf x = \psi (x) = (1,x,x^2,...,x^n)$$

$$\mathbf a = (a_0, a_1,...a_n)$$

那么，

$$\mathcal H = \langle \mathbf a, \mathbf x \rangle$$

与\(1\)式相同，于是问题转化为线性回归问题。



