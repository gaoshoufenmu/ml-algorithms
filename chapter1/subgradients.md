# 次梯度

前面我们讲梯度下降时要求目标函数$$f$$连续可微，现在我们将目标函数泛化，对于不可微的情况，使用另一种方法，也即本篇要讨论的次梯度。

先回想一下前面提到的凸函数有以下性质，

$$\forall \mathbf u, f(\mathbf u) \ge f(\mathbf w) + \langle \mathbf u - \mathbf w, \nabla f(\mathbf w) \rangle$$

扩展一下，令$$S$$ 是开凸集，函数$$f: S \rightarrow \Bbb R$$ 是凸函数当且仅当对$$\forall \mathbf w \in S, \exists \mathbf v$$ 满足：

$$\forall \mathbf u \in S, \quad f(\mathbf u) \ge f(\mathbf w) + \langle \mathbf u - \mathbf w, \mathbf v \rangle$$

这里略去证明，直接使用这条引理。满足这条引理的$$\mathbf v$$ 就是函数$$f$$ 在点$$\mathbf w$$ 处的次梯度，次梯度集合称为可微集，记作$$\partial f(\mathbf w)$$

上述概率理解起来不难，本质上来说，次梯度就是位于凸函数$$f$$ （不一定可微）下方且经过点$$\mathbf w$$ 的直线的斜率



