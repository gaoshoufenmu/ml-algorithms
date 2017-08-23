# Gradient Descent

假设要求一个连续可微凸函数的最小值：$$min \quad f(\mathbf w) $$，其中$$f: \mathbb R^d \rightarrow \mathbb R$$，位于$$\mathbf w$$ 处的梯度为

$$\nabla f(\mathbf w) = (\frac {\partial f(\mathbf w)} {\partial \mathbf w_1}, ..., \frac {\partial f(\mathbf w)} {\partial \mathbf w_d})$$

假设总共需要迭代$$T$$ 次，每次更新使用以下公式

$$\mathbf w^{(t+1)} = \mathbf w^{(t)} - \eta \nabla f (\mathbf w^{(t)})$$

其中$$\eta \ge 0$$  表示步长。这样迭代结束后使用$$\overline {\mathbf w}= \frac 1 T \sum_{t=1}^T \mathbf w^{(t)}$$ 作为最后的输出，当然也可以使用最后一步的$$\mathbf w^{(T)}$$ 或者使用$$T$$ 次中使目标函数最小的$$argmin_{t \in [T]}f(\mathbf w^{(t)})$$ 作为最后输出，不过通常都是使用平均向量$$\overline {\mathbf w}$$在实际应用中更好一些，尤其是处理非连续可微的目标函数。

我们也可以从另一个方面来理解，对目标函数$$f$$ 采用一级泰勒展开，可以得到

$$f(\mathbf u) \approx f(\mathbf w) + \langle \mathbf u - \mathbf w, \nabla f (\mathbf w) \rangle$$

因为假定$$f$$ 是凸函数，函数位于切平面（1维情况就是切线）上方，所以有

$$f(\mathbf u) \ge f(\mathbf w) + \langle \mathbf u - \mathbf w, \nabla f (\mathbf w) \rangle$$

令$$\mathbf w$$ 逐渐逼近$$\mathbf w^{(t)}$$，于是 $$f(\mathbf w) \approx f(\mathbf w^{(t)}) + \langle \mathbf w - \mathbf w^{(t)}, \nabla f (\mathbf w^{(t)}) \rangle$$，我们通过迭代选取一个新的$$\mathbf w$$值，使这个式子的右端变小，从而使得$$f(\mathbf w)$$变小，这正与我们的求解目的吻合，但是$$\mathbf w$$ 与 $$\mathbf w^{(t)}$$ 的距离值得一番思考，距离太小会使得$$f(\mathbf w)$$ 变小不明显，增加迭代次数，从而增加计算复杂度，而距离太大则可能会跳过最小值点，所以需要找到一个平衡点，一方面使得距离不能太大，另一方面又要使$$f(\mathbf w)$$ 变小，我们增加一个变量$$\eta$$ 来平衡这两者，然后再求最优的$$\mathbf w$$ 的解，如下

$$\mathbf w^{(t+1)} = argmin_{\mathbf w} \quad \frac 1 2 \left\| \mathbf w - \mathbf w^{(t)} \right\|^2 + \eta (f(\mathbf w^{(t)}) + \langle \mathbf w - \mathbf w^{(t)}, \nabla f (\mathbf w^{(t)})\rangle )$$

也许有人不理解为什么是求最小值，为什么不是求最大值呢？其实这里求最大值还是最小值都是等价的，我们不难理解上式等价于以下

$$\mathbf w^{(t+1)} = argmax_{\mathbf w} \quad - \frac 1 2 \left\| \mathbf w - \mathbf w^{(t)} \right\|^2 - \eta (f(\mathbf w^{(t)}) + \langle \mathbf w - \mathbf w^{(t)}, \nabla f (\mathbf w^{(t)})\rangle )$$

令$$\gamma = - \eta$$，于是

$$\mathbf w^{(t+1)} = argmax_{\mathbf w} \quad - \frac 1 2 \left\| \mathbf w - \mathbf w^{(t)} \right\|^2 + \gamma (f(\mathbf w^{(t)}) + \langle \mathbf w - \mathbf w^{(t)}, \nabla f (\mathbf w^{(t)})\rangle )$$

