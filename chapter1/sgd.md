## 随机梯度下降

上一篇文章中我们将目标函数从可微条件扩展到更一般的非可微情况，并引出次梯度（集合）概念，且知道了对于可微的情况，梯度也属于一种特殊的次梯度，即次梯度集合中只有一个元素，就是可微函数在点$$\mathbf w$$ 处的梯度。然而，有时候我们在迭代过程中可能并不准确地根据梯度下降的方向来更新$$\mathbf w$$，而是从一个分布中随机选择一个值来作为次梯度，这个分布的期望等于$$\mathbf w$$ 处的次梯度。我们将随机梯度下降简称微SGD

于是，

令$$B, \rho \gt 0, \mathbf w^* \in argmin_{\mathbf w: \left\| \mathbf w \right\| \le B} f(\mathbf w)$$，目标函数$$f$$ 是凸函数，SGD迭代步长$$\eta = \sqrt {\frac {B^2} {\rho^2 T}}$$，且假定随机选择作为次梯度的向量$$\mathbf v_t$$ 的$$L_2$$ 范数以概率1 小于$$\rho$$，那么有，

$$\Bbb E [f(\overline {\mathbf w})] - f(\mathbf w^*) \le \frac {B \rho} {\sqrt T}$$

令上面这个误差不超过$$\epsilon$$，于是，

$$\frac {B \rho} {\sqrt T} \le \epsilon$$

从而有$$T \ge \frac {B^2 \rho^2} {\epsilon^2}$$

变形



