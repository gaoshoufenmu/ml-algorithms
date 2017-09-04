强凸函数满足：

$$\langle \mathbf w - \mathbf u, \mathbf v \rangle \ge f(\mathbf w) - f(\mathbf u) + \frac \lambda 2 \left\| \mathbf w - \mathbf u \right\|^2$$

于是针对强凸函数的求最小值的SGD步骤如下，

1. 目标：求 $$min_{\mathbf w \in \mathcal H} f(\mathbf w)$$
2. 输入：$$\mathbf w^{(1)} = \mathbf 0, \quad T$$
3. 迭代：$$\mathbf {for} \quad t = 1,...,T$$
4. 对期望$$\Bbb E [\mathbf v_t|\mathbf w^{(t)}] \in \partial f (\mathbf w^{(t)})$$的分布，随机选择一个向量$$\mathbf v_t$$
5. 令$$\eta_t = 1 / (\lambda t), \quad \mathbf w^{(t+1/2)} = \mathbf w^{(t)} - \eta_t \mathbf v_t, \quad \mathbf w^{(t+1)} = argmin_{\mathbf w \in \mathcal H} \left\| \mathbf w - \mathbf w^{(t+1/2)} \right\|^2$$
6. 继续执行步骤4，直到$$t=T$$ 后退出循环迭代
7. 输出：$$\overline {\mathbf w} = \frac 1 T \sum_{t=1}^T \mathbf w^{(t)}$$

定理

假设$$f$$ 是$$\lambda$$ 强凸函数，并且$$\Bbb E [\left\| \mathbf v_t \right\|^2] \le \rho^2$$，令$$\mathbf w^* \in argmin_{\mathbf w \in \mathcal H} f(\mathbf w)$$ 为$$\mathcal H$$ 上的一个最优解，那么，

$$\Bbb E[f(\overline {\mathbf w})] - f(\mathbf w^*) \le \frac {\rho^2} {2 \lambda T} (1 + log(T))$$

