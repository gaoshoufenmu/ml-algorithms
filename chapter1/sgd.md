## 随机梯度下降

上一篇文章中我们将目标函数从可微条件扩展到更一般的非可微情况，并引出次梯度（集合）概念，且知道了对于可微的情况，梯度也属于一种特殊的次梯度，即次梯度集合中只有一个元素，就是可微函数在点$$\mathbf w$$ 处的梯度。然而，有时候我们在迭代过程中可能并不准确地根据梯度下降的方向来更新$$\mathbf w$$，而是从一个分布中随机选择一个值来作为次梯度，这个分布的期望等于$$\mathbf w$$ 处的次梯度。我们将随机梯度下降简称微SGD

于是，

令$$B, \rho \gt 0, \mathbf w^* \in argmin_{\mathbf w: \left\| \mathbf w \right\| \le B} f(\mathbf w)$$，目标函数$$f$$ 是凸函数，SGD迭代步长$$\eta = \sqrt {\frac {B^2} {\rho^2 T}}$$，且假定随机选择作为次梯度的向量$$\mathbf v_t$$ 的$$L_2$$ 范数以概率1 小于$$\rho$$，那么有，

$$\Bbb E [f(\overline {\mathbf w})] - f(\mathbf w^*) \le \frac {B \rho} {\sqrt T}$$

令上面这个误差不超过$$\epsilon$$，于是，

$$\frac {B \rho} {\sqrt T} \le \epsilon$$

从而有$$T \ge \frac {B^2 \rho^2} {\epsilon^2}$$

* ### 变形

前面介绍GD和SGD时，我们做了一个限定$$\mathcal H = \lbrace \mathbf w: \left\| \mathbf w \right\| \le B \rbrace$$，即在一个范围内搜索最优值的点，这是为了保障根据GD或者SGD迭代$$T$$ 次后的解与真实的最优解的在给定误差内，然而这个限定却不一定能满足，首先解释GD情况，如果有$$\mathbf w^* = argmax_{\mathbf w \in \mathcal H}  \left\| \mathbf w \right\|$$，那么在某个很靠近$$\mathbf w^*$$的$$\mathbf w^{(t)}$$时，搞不好此时的更新迭代就使得$$\mathbf w^{(t+1)}$$跨过$$\mathbf w^*$$从而使得$$\left\| \mathbf w^{(t+1)} \right\| \gt \left\| \mathbf w^* \right\|$$。SGD的情况下就更加无法保证这个限定条件了。

* #### 投影

一个直接的解决方法就是，既然迭代后有可能$$\left\| \mathbf w^{(t+1)} \right\| \notin \mathcal H$$，那就从$$\mathcal H$$ 中找一个与$$\mathbf w^{(t+1)}$$ 最靠近的点，即，将点投影到$$\mathcal H$$ 上，于是更新操作分为两步：

1. 原先的更新操作，$$\mathbf w^{(t+1/2)} = \mathbf w^{(t)} - \eta \mathbf v_t$$
2. 投影操作，$$\mathbf w^{(t+1)} = argmin_{\mathbf w \in \mathcal H} \left\| \mathbf w - \mathbf w^{(t + 1/2)} \right\|$$

由于$$\forall \mathbf u \in \mathcal H, \left\| \mathbf w^{(t+1/2)} - \mathbf u \right\|^2 \ge \left\| \mathbf w^{(t+1)} - \mathbf u \right\|^2$$（证明略），所以有

$$\left\| \mathbf w^{(t+1)} - \mathbf w^* \right\|^2 - \left\| \mathbf w^{(t)} - \mathbf w^* \right\|^2 \le \left\| \mathbf w^{(t+1/2)} - \mathbf w^* \right\|^2 - \left\| \mathbf w^{(t)} - \mathbf w^* \right\|^2$$

我们仿照GD的分析中的证明，有

$$\begin{aligned} \langle \mathbf w^{(t)} - \mathbf w^*, \mathbf v_t \rangle & = \frac 1 \eta \langle \mathbf w^{(t)} - \mathbf w^*, \eta \mathbf v_t \rangle \\ & = \frac 1 {2 \eta} (- \left\| \mathbf w^{(t)} - \mathbf w^* - \eta \mathbf v_t \right\|^2 + \left\| \mathbf w^{(t)} - \mathbf w^* \right\|^2 + \eta^2 \left\| \mathbf v_t \right\|^2) \\ & = \frac 1 {2 \eta} (- \left\| \mathbf w^{(t+1/2)} - \mathbf w^* \right\|^2 + \left\| \mathbf w^{(t)} - \mathbf w^* \right\|^2) + \frac \eta 2 \left\| \mathbf v_t \right\|^2 \\ & \le \frac 1 {2 \eta} (- \left\| \mathbf w^{(t+1)} - \mathbf w^* \right\|^2 + \left\| \mathbf w^{(t)} - \mathbf w^* \right\|^2) + \frac \eta 2 \left\| \mathbf v_t \right\|^2 \end{aligned}$$

剩下的，已无需赘述，不难发现加上投影操作后依然有

$$\sum_{t=1}^T \langle \mathbf w^{(t)} - \mathbf w^*, \mathbf v_t \rangle \le \frac 1 {2 \eta} \left\| \mathbf w^* \right\|^2 + \frac \eta 2 \sum{t=1}^T \left\| \mathbf vt \right\|^2$$

于是迭代后误差的计算与前面一致。

* 变步长

改变每次迭代的步长，即令$$\eta = \frac B {\rho \sqrt t}$$，这样在接近最优解$$\mathbf w^*$$ 的时候，此时$$t$$ 越来越大，步长就越来越小，我们的迭代变得很小心谨慎，以防止跨越$$\mathbf w^*$$（太多）

