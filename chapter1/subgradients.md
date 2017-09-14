## 次梯度

前面我们讲梯度下降时要求目标函数$$f$$连续可微，现在我们将目标函数**泛化**，对于不可微的情况，使用另一种方法，也即本篇要讨论的次梯度。

先回想一下前面提到的凸函数有以下性质，

$$\forall \mathbf u, f(\mathbf u) \ge f(\mathbf w) + \langle \mathbf u - \mathbf w, \nabla f(\mathbf w) \rangle$$

扩展一下，令$$S$$ 是开凸集，函数$$f: S \rightarrow \Bbb R$$ 是凸函数当且仅当对$$\forall \mathbf w \in S, \exists \mathbf v$$ 满足：

$$\forall \mathbf u \in S, \quad f(\mathbf u) \ge f(\mathbf w) + \langle \mathbf u - \mathbf w, \mathbf v \rangle$$                             $$(1)$$

这里略去证明，直接使用这条引理。满足这条引理的$$\mathbf v$$ 就是函数$$f$$ 在点$$\mathbf w$$ 处的次梯度，次梯度集合称为可微集，记作$$\partial f(\mathbf w)$$

上述概率理解起来不难，本质上来说，次梯度就是不位于凸函数$$f$$ （不一定可微）上方且经过点$$\mathbf w$$ 的直线的斜率（向量），那么问题来了，给定目标函数$$f$$，如何计算其次梯度集合？

显然对于可微函数，经过点$$\mathbf w$$ 只有切线方向的斜率满足要求，其他直线均会与$$f$$ 曲线相交从而无法满足“位于函数曲线下方”这个要求，所以$$\partial f(\mathbf w)$$ 仅包含一个元素即$$\mathbf w$$ 处的梯度 $$\nabla f(\mathbf w)$$

对于不连续可微函数，我们首先以绝对值函数作为一个例子：$$f(x)=|x|$$，从下图中不难看出，

![](/assets/subgradients.png)

$$\partial f(x) = \begin{cases} \lbrace 1 \rbrace &  \quad x \gt 0 \\ \lbrace -1 \rbrace &  \quad x \lt 0 \\ \left[ -1,1 \right] &  \quad x = 0 \end{cases}$$

* #### 获取次梯度的方法

实际上，对于分段凸函数$$g(\mathbf w) = max_{i \in [r]} g_i(\mathbf w)$$，它是由$$r$$ 个可微凸函数在每点处去最大值组成，假设在$$\mathbf w$$ 处，$$g_j(\mathbf w)$$ 取得最大值，必然有$$\nabla g_j(\mathbf w) \in \partial g(\mathbf w)$$，这是因为其切线位于$$g_j(\mathbf w)$$下方，而$$g(\mathbf w)$$ 取各个可微函数中的最大值，所以此切线必然也位于$$g(\mathbf w)$$ 的下方。

总结一下，我们可以将可微函数的梯度扩展到非可微函数的次梯度，这样，就可以继续使用梯度下降法了，虽然次梯度是一个集合，但是计算时只要其中一个次梯度值即可。

* 利普希茨函数

函数$$f: A \rightarrow \Bbb R$$，如果对$$\forall \mathbf u, \mathbf v \in A$$，均有 $$|f(\mathbf u) - f(\mathbf v)| \le \rho \left\| \mathbf u - \mathbf v \right\|$$，那么就称此函数在$$A$$ 上满足利普希茨条件

显然，由于$$\mathbf u, \mathbf v$$ 的任意性，可知$$\forall \mathbf w \in A, \mathbf v \in \partial f(\mathbf w)$$，均有$$\left\| \mathbf v \right\| \le \rho$$

