# Gradient Descent

假设要求一个连续可微凸函数的最小值：$$min \quad f(\mathbf w) $$，其中$$f: \mathbb R^d \rightarrow \mathbb R$$，位于$$\mathbf w$$ 处的梯度为

$$\nabla f(\mathbf w) = (\frac {\partial f(\mathbf w)} {\partial \mathbf w_1}, ..., \frac {\partial f(\mathbf w)} {\partial \mathbf w_d})$$

假设总共需要迭代$$T$$ 次，每次更新使用以下迭代公式

$$\mathbf w^{(t+1)} = \mathbf w^{(t)} - \eta \nabla f (\mathbf w^{(t)})$$

其中$$\eta \ge 0$$  表示步长。这样迭代结束后使用$$\overline {\mathbf w}= \frac 1 T \sum_{t=1}^T \mathbf w^{(t)}$$ 作为最后的输出，当然也可以使用最后一步的$$\mathbf w^{(T)}$$ 或者使用$$T$$ 次中使目标函数最小的$$argmin_{t \in [T]}f(\mathbf w^{(t)})$$ 作为最后输出，不过通常都是使用平均向量$$\overline {\mathbf w}$$在实际应用中更好一些，尤其是处理非连续可微的目标函数。

我们也可以从另一个方面来理解，对目标函数$$f$$ 采用一级泰勒展开，可以得到

$$f(\mathbf u) \approx f(\mathbf w) + \langle \mathbf u - \mathbf w, \nabla f (\mathbf w) \rangle$$

因为假定$$f$$ 是凸函数，函数位于切平面（1维情况就是切线）上方，所以有

$$f(\mathbf u) \ge f(\mathbf w) + \langle \mathbf u - \mathbf w, \nabla f (\mathbf w) \rangle$$

令$$\mathbf w$$ 逐渐逼近$$\mathbf w^{(t)}$$，于是 $$f(\mathbf w) \approx f(\mathbf w^{(t)}) + \langle \mathbf w - \mathbf w^{(t)}, \nabla f (\mathbf w^{(t)}) \rangle$$，我们通过迭代选取一个新的$$\mathbf w$$值，使这个式子的右端变小，从而使得$$f(\mathbf w)$$变小，这正与我们的求解目的吻合，然而，式子右端减小可能会使得$$\mathbf w$$ 与 $$\mathbf w^{(t)}$$ 的距离增大，而距离太大则可能会跳过最小值点，从而形成一个震荡，使得一直达不到最小值点，所以需要找到一个平衡点，一方面使得距离更小，另一方面又要使$$f(\mathbf w)$$ 更小，如下图所示，

![](/assets/iter.png)

我们增加一个变量$$\eta$$ 来平衡这两者的大小倍率关系，然后再求最优的$$\mathbf w$$ 的解，如下

$$\mathbf w^{(t+1)} = argmin_{\mathbf w} \quad \frac 1 2 \left\| \mathbf w - \mathbf w^{(t)} \right\|^2 + \eta (f(\mathbf w^{(t)}) + \langle \mathbf w - \mathbf w^{(t)}, \nabla f (\mathbf w^{(t)})\rangle )$$

将上式看作关于$$\mathbf w$$ 的方程求最小值，对$$\mathbf w$$ 求导并令其等于0，

$$\mathbf w - \mathbf w^{(t)} + \eta \nabla f(\mathbf w^{(t)}) = 0 $$

于是下一步迭代使用的$$\mathbf w$$ 为，

$$\mathbf w^{(t+1)} = \mathbf w = \mathbf w^{(t)} - \eta \nabla f(\mathbf w^{(t)})$$

