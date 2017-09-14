强凸函数满足：$$\forall \mathbf w, \mathbf u, \mathbf v \in \partial f(\mathbf w)$$

$$\langle \mathbf w - \mathbf u, \mathbf v \rangle \ge f(\mathbf w) - f(\mathbf u) + \frac \lambda 2 \left\| \mathbf w - \mathbf u \right\|^2$$

称函数为$$\lambda$$ 强函数

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

证明：

令$$\nabla^{(t)} = \Bbb E [\mathbf v_t | \mathbf w^{(t)}]$$表示关于给定点$$\mathbf w^{(t)}$$ 的随机变量为$$\mathbf v_t$$ 的期望，且属于$$\mathbf w^{(t)}$$处的可微集，即 $$\nabla^{(t)} \in \partial f(\mathbf w^{(t)})$$，由于目标函数$$f$$ 是强凸函数，根据强凸函数的性质有，

$$\langle \mathbf w^{(t)} - \mathbf w^*, \nabla^{(t)} \rangle \ge f(\mathbf w^{(t)}) - f(\mathbf w^*) + \frac \lambda 2 \left\| \mathbf w^{(t)} - \mathbf w^* \right\|^2$$     \(\*\)

由于$$\mathbf w^{(t+1)}$$ 是$$\mathbf w^{(t+1/2)}$$ 在$$\mathcal H$$ 上的投影，且$$\mathbf w^* \in \cal H$$，所以有$$\left\| \mathbf w^{(t+1/2)} - \mathbf w^* \right\|^2 \ge \left\| \mathbf w^{(t+1)} - \mathbf w^* \right\|^2$$，因此，

$$\left\| \mathbf w^{(t)} - \mathbf w^* \right\|^2 - \left\| \mathbf w^{(t+1)} - \mathbf w^* \right\|^2 \ge \left\| \mathbf w^{(t)} - \mathbf w^* \right\|^2 - \left\| \mathbf w^{(t+1/2)} - \mathbf w^* \right\|^2 $$

再根据迭代公式 $$\mathbf w^{(t+1/2)} = \mathbf w^{(t)} - \eta \mathbf v_t$$，于是，

$$\left\| \mathbf w^{(t)} - \mathbf w^* \right\|^2 - \left\| \mathbf w^{(t+1)} - \mathbf w^* \right\|^2 \ge 2 \eta_t \langle \mathbf w^{(t)} - \mathbf w^* , \mathbf v_t \rangle - \eta_t^2 \left\| \mathbf v_t \right\|^2$$

假设$$\Bbb E [\left\| \mathbf v_t \right\|^2] \le \rho^2$$，于是对上式取期望可得，

$$\langle \mathbf w^{(t)} - \mathbf w^* , \mathbf v_t \rangle \le \frac {\Bbb E [\left\| \mathbf w^{(t)} - \mathbf w^* \right\|^2 - \left\| \mathbf w^{(t+1)} - \mathbf w^* \right\|^2]} {2 \eta_t} + \frac {\eta_t} 2 \rho^2$$

结合\(\*\)式则，

$$\sum_{t=1}^T (\Bbb E [f(\mathbf w^{(t)})] - f(\mathbf w^*)) \le \Bbb E [\sum_{t=1}^T (\frac {\left\| \mathbf w^{(t)} - \mathbf w^* \right\|^2 - \left\| \mathbf w^{(t+1)} - \mathbf w^* \right\|^2} {2 \eta_t} - \frac \lambda 2 \left\| \mathbf w^{(t)} - \mathbf w^* \right\|^2)] + \frac {\rho^2} 2 \sum{t=1}^T \eta_t$$

现在令$$\eta_t = 1/(\lambda t)$$，并且注意到上式右端第一项的求和，展开让相邻项相消，可得 $$- \lambda T \left\| \mathbf w^{(T+1)} - \mathbf w^* \right\|^2 \le 0 $$，所以有，

$$\sum_{t=1}^T (\Bbb E [f(\mathbf w^{(t)})] - f(\mathbf w^*)) \le \frac {\rho^2} {2 \lambda} \sum_{t=1}^T \frac 1 t \le \frac {\rho^2} {2 \lambda} (1 + log(T))$$

根据凸函数的性质，利用Jensen不等式，得证。

