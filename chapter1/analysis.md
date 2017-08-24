上一篇文章中，我们知道了梯度下降法求解目标函数的最小值，以及给出了迭代公式，然而迭代$$T$$ 次后到底离目标函数真正的最小值还差多少？本篇文章我们将来分析这个问题。

设目标函数$$f(\mathbf w)$$ 在$$\mathbf w^*$$ 处取得最小值，梯度下降法迭代$$T$$ 次后的输出为$$\overline {\mathbf w} = \frac 1 T \sum_{t=1}^T \mathbf w^{(t)}$$，于是，

$$\begin{align} f(\overline {\mathbf w}) - f(\mathbf w^*) & = f(\frac 1 T \sum_{t=1}^T \mathbf w^{(t)}) - f(\mathbf w^*)  \\ & \le \frac 1 T \sum_{t=1}^T (f(\mathbf w^{(t)})) - f(\mathbf w^*) \\ & = \frac 1 T \sum_{t=1}^T (f(\mathbf w^{(t)}) - f(\mathbf w^*)) \end{align}$$

由于目标函数$$f(\mathbf w)$$ 是下凸的，所以上面不等式成立，且有下面的性质（可以理解为过点$$\mathbf w^{(t)}$$的切线位于函数曲线下方，所以按切线方向下降的比函数下降的快，如下图），

$$f(\mathbf w^{(t)}) - f(\mathbf w^*) \le \langle \mathbf w{(t)} - \mathbf w^*, \nabla f(\mathbf w^{(t)}) \rangle$$

![](/assets/decent_cmp.png)

结合上面两式有，

$$f(\overline {\mathbf w}) - f(\mathbf w^*) \le \frac 1 T \sum_{t=1}^T \langle \mathbf w^{(t)} - \mathbf w^*, \nabla f(\mathbf w^{(t)}) \rangle$$                          $$(*)$$

现在我们需要证明：

$$\mathbf w^{(t+1)} = \mathbf w{(t)} - \eta \mathbf v_t \Rightarrow \sum_{t=1}^T \langle \mathbf w^{(t)} - \mathbf w^*, \mathbf v_t \rangle \le \frac {\left\| \mathbf w^* \right \|^2} {2 \eta} + \frac \eta 2 \sum_{t=1}^T \left\| \mathbf v_t \right\|^2$$

其中，$$\mathbf w^{(1)} = \mathbf 0, \mathbf v_t$$是任意向量。

证：

$$\begin{align} \langle \mathbf w^{(t)} - \mathbf w^*, \mathbf v_t \rangle & = \frac 1 \eta \langle \mathbf w^{(t)} - \mathbf w^*, \eta \mathbf v_t \rangle \\ & = \frac 1 {2 \eta} (- \left\| \mathbf w^{(t)} - \mathbf w^* - \eta \mathbf v_t \right\|^2 + \left\| \mathbf w^{(t)} - \mathbf w^* \right\|^2 + \eta^2 \left\| \mathbf v_t \right\|^2) \\ & = \frac 1 {2 \eta} (- \left\| \mathbf w^{(t+1)} - \mathbf w^* \right\|^2 + \left\| \mathbf w^{(t)} - \mathbf w^* \right\|^2) + \frac \eta 2 \left\| \mathbf v_t \right\|^2 \end{align}$$

上式第二步通过添项补全形成平方和，第三步使用迭代公式，然后对上式求和，消去相同项得到，

$$\begin{align} \sum_{t=1}^T \langle \mathbf w^{(t)} - \mathbf w^*, \mathbf v_t \rangle & =  \frac 1 {2 \eta} (\left\| \mathbf w^{(1)} - \mathbf w^* \right\|^2 - \left\| \mathbf w^{(T+1)} - \mathbf w^* \right\|^2) + \frac \eta 2 \sum_{t=1}^T \left\| \mathbf v_t \right\|^2 \\ & \le \frac 1 {2 \eta} \left \| \mathbf w^{(1)} - \mathbf w^* \right\|^2 + \frac \eta 2 \sum{t=1}^T \left\| \mathbf v_t \right\|^2 \\ & = \frac 1 {2 \eta} \left\| \mathbf w^* \right\|^2 + \frac \eta 2 \sum_{t=1}^T \left\| \mathbf v_t \right\|^2 \end{align}$$

于是，令$$\mathbf v_t = \nabla f(\mathbf w^{(t)})$$，并假定$$ \left\| \mathbf w^* \right\|^2 \le B, \left\| \mathbf v_t \right\| \le \rho$$，

令$$\eta = \sqrt {\frac {B^2} {\rho^2 T}}$$，故，

$$\frac 1 T \sum_{t=1}^T \langle \mathbf w^{(t)} - \mathbf w^*, \mathbf v_t \rangle \le \frac {B \rho} {\sqrt T}$$

结合上面$$(*)$$式，

$$f(\overline {\mathbf w}) - f(\mathbf w^*) \le \frac {B \rho} {\sqrt T}$$

如果我们希望误差不超过指定值$$\epsilon$$，即

$$\frac {B \rho} {\sqrt T} \le \epsilon$$

从而迭代次数满足：

$$ T \ge \frac {B^2 \rho^2} {\epsilon^2}$$

