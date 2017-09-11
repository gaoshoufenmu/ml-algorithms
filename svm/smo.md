本篇文章承接上一篇文章Solve，本篇文章中的方程序号也跟随上一篇文章的方程序号而不是从头开始。

软间隔

现实中的样本并非一定是线性可分的，此时，可以放宽限制条件，本章第一篇文章中的\(6\)式中的限制条件为$$\forall i \in [m]， y_i(\mathbf {wx}_i + b) \ge 1$$，这表示所有点能被超平面正确划分，等号成立时的样本点位于间隔边界上，大于号成立时的样本点位于间隔边界两侧。对于线性不可分的样本，我们引入松弛变量\(slack variable\) $$\xi_i \ge 0$$，将限制条件改为$$\forall i \in [m]， y_i(\mathbf {wx}_i + b) \ge 1 - \xi_i$$，允许一定数量的带内样本点或误分类点，根据本章第一篇文章中的\(5\)式知道带内宽度为$$\frac 2 {|\mathbf w|}$$，划分超平面位于带内中间位置，如下图，

![](/assets/SVM_soft.png)

图中直观地显示了$$\xi_i$$ 的大小表示对误分类点$$(\mathbf x_i, y_i)$$容忍程度的强弱，$$\frac {\xi_i} {|\mathbf w|}$$ 表示误分类点远离对应间隔边界的距离。

由于放宽了限制条件，相应的本章第一篇文章中\(6\)式的目标函数$$\frac 1 2 |\mathbf w|^2$$也需要增加一项惩罚因子，不然因为引入松弛因子，再小的$$|\mathbf w|$$都是允许的（此时虽然足够小的$$|\mathbf w|$$ 使得间距足够大，即便是所有样本点均处于带内或者成为误分类点，由于松弛因子的存在，这样做都是满足约束条件的，但显然背离了我们的目标），显然，我们虽然放宽了约束条件，但是我们并不会放纵那些误分类点距离对应的间隔边界太远，也就是说，只能容忍较小程度的误分类，不希望$$\xi_i$$太大，所以目标函数的优化改写为，

$$min_{\mathbf w, b, \mathbf \xi} \ \frac 1 2 |\mathbf w|^2 + C \sum_{i=1}^m \xi_i, \quad \forall i \in [m]， y_i(\mathbf {wx}_i + b) \ge 1 - \xi_i, \ \xi_i \ge 0$$          \(5\)

其中$$C$$ 是带内间距和误分类点之间的平衡因子，$$C$$ 越大，对误分类点的惩罚就越大。对$$C$$ 不做约束，实际计算中根据经验指定为一个固定值。

对偶算法

将问题\(5\)转为二次规划标准形，拉格朗日函数为，

$$L(\mathbf w, b, \xi, \alpha, \mu) = \frac 1 2 |\mathbf w|^2 + C \sum_ {i=1}^m \xi_i + \sum_ {i=1}^m \alpha_i [1 - \xi_i - y_i (\mathbf {wx}_i + b)] - \sum_{i=1}^m \mu_i \xi_i$$

根据本章第二篇文件对拉格朗日对偶性的介绍，此处对偶问题为，

$$max_{\alpha, \mu} \ min_{\mathbf w, b, \xi} \ L$$

求偏导并令其等于0 如下（$$\mathbf x_{ki}$$ 表示第$$k$$ 个样本点输入向量的第$$i$$ 个分量），

$$\nabla_{\mathbf w_i} L = \mathbf w_i - \sum_{k=1}^m \alpha_k y_k \mathbf x_{ki} = 0 , \quad \forall i \in [m]$$

$$\nabla_b L = - \sum_{k=1}^m \alpha_k y_k = 0$$

$$\nabla_{\xi_i} L = C - \alpha_i - \mu_i = 0, \quad \forall i \in [m]$$

解得（以向量形式表示），

$$\begin{cases} \mathbf w = \sum_{i=1}^m \alpha_i y_i \mathbf x_i \\ \sum_{i=1}^m \alpha_i y_i = 0 \\ C - \alpha_i - \mu_i = 0 \end{cases}$$

代入$$L$$  得，

$$L = - \frac 1 2 \sum_{i=1}^m \sum_{j=1}^m \alpha_i \alpha_j y_i y_j \mathbf x_i \mathbf x_j + \sum_{i=1}^m \alpha_i $$

然后将上式对$$\alpha$$ 求极大，注意到乘子$$\mu_i \ge 0$$，所以有，

$$0 \le \alpha_i \le C$$

整理为，

$$min_{\alpha} \ \frac 1 2 \sum_{i=1}^m \sum_{j=1}^m \alpha_i \alpha_j y_i y_j \mathbf x_i \mathbf x_j - \sum_{i=1}^m \alpha_i$$

$$\sum_{i=1}^m y_i \alpha_i = 0$$

$$0 \le \alpha_i \le C, \ \forall i \in [m]$$

我们给出\(5\)的KKT条件，

$$\alpha_i \ge 0, \quad \mu_i \ge 0$$                   \(拉格朗日乘子的限制条件\)

$$y_i f(\mathbf x_i) - 1 + \xi_i \ge 0$$        （原始优化问题的约束条件）

$$\alpha_i [y_i f(\mathbf x_i) - 1 + \xi_i] = 0$$  （强对偶性条件）

$$\mu_i \xi_i = 0$$                                 （强对偶性条件）

$$\xi_i \ge 0 $$                                     （软间隔约束放宽条件）

根据以上5个条件，可以得出以下结论（读者可以自己推导），

$$\begin {cases} \alpha_i = 0 \Leftrightarrow y_i f(\mathbf x_i) \ge 1 \\ 0 \lt \alpha_i \lt C \Leftrightarrow y_i f(\mathbf x_i) = 1 \\ \alpha_i = C \Leftrightarrow y_i f(\mathbf x_i)  \le 1 \end{cases}$$

#### 解析解

根据上一篇文章\(3\)式约束条件有，

$$\alpha_i y_i + \alpha_j y_j = c, \quad \alpha_i \ge 0, \alpha_j \ge 0$$                                                                                   \(5\)

其中常数（在当前选择$$\alpha_i, \alpha_j$$ 这两个变量时，其他乘子都被固定，可以看作常数）为，

$$c = - \sum_{k \neq i,j} \alpha_k y_k$$

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

