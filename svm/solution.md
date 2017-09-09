上一篇文章我们知道了求解超平面$$\mathbf {wx} + b = 0$$ 的问题描述

$$argmin_{\mathbf w, b} \frac 1 2 |\mathbf w|^2 \quad s.t. \forall i \in [m]， y_i(\mathbf {wx}_i + b) \ge 1$$                                                  \(1\)

显然这是一个凸二次规划问题。

求解约束最优化问题，我们通常利用拉格朗日对偶性，将原问题转化为对偶问题，因为这样求解效率更高。这里又双叒叕给懒人福利了，如果还不了解拉格朗日乘子法，那么可以暂时不用专门去查资料，这里将简单介绍一下（真的很简单地介绍呀喂~）

* 拉格朗日乘子法

设目标函数$$f(x)$$，等式约束$$h(x)$$，不等式约束$$g(x)$$，优化问题为，

$$min \quad f(x)$$

$$h_j(x) = 0, \quad \forall j \in [p]$$

$$g_k(x) \le 0, \quad \forall k \in [q]$$

那么，给每个约束条件添加拉格朗日乘子，注意特别要求$$\mu_k \ge 0$$，并与目标函数相加得到拉格朗日函数，

$$L(x, \lambda, \mu) = f(x) + \sum_{j=1}^p \lambda_j h_j(x) + \sum_{k=1}^q \mu_k g_k(x)$$

于是，问题变为函数$$L$$ 的求极值，具体来说，就是先对$$\lambda, \mu$$ 求极大值，然后再对$$x$$ 求极小值。

有些人肯定会这么想：你要给目标函数$$f(x)$$如此变换成拉格朗日函数$$L$$ 我没意见，但是凭什么说对这个$$L$$ 如此求极值就等价于原优化问题呢？万一不等价，那求$$L$$ 的极值有个球用？

好吧，为了能顺利的愉快的阅读下去，这个心结还是要解开才行。

令$$\theta(x) = max_{\lambda, \mu} L(x, \lambda, \mu)$$，对$$\lambda, \mu$$ 求$$L$$ 的最大值，我们要牢牢记住，$$\theta(x)$$ 表示的是每个$$x$$ 取值下的$$L$$ 最大值。于是$$\theta(x)$$ 就只和$$x$$ 有关了，然后我们分析$$x$$ 的取值对函数$$\theta(x)$$ 的影响，

\(1\) 若$$x$$ 不满足原约束条件，即存在 $$h_j(x) \neq 0 \quad or \quad g_k(x)  \gt 0$$，那么，

$$\theta(x) = max_{\lambda, \mu} [f(x) + \sum_{j=1}^p \lambda_j h_j(x) + \sum_{k=1}^q \mu_k g_k(x)] = + \infty$$

这是为什么呢？假设 $$\exists j, h_{j}(x) \neq 0$$，那么当取值 $$\lambda_j \rightarrow \infty$$，且正负性与$$h_j(x)$$ 相同，就有$$\lambda_j h_j(x) \rightarrow +\infty$$，而如果$$\exists k, g_k(x) \gt 0$$，那么当取值 $$\mu_k \rightarrow +\infty$$，就有$$\mu_k g_k(x) \rightarrow +\infty$$，所以此情况下$$\theta(x) = +\infty$$

\(2\) 若$$x$$ 满足原约束条件，此时$$f_j(x) = 0$$，所以，

$$\theta(x) = max_{\lambda, \mu} [f(x) + \sum_{j=1}^p \lambda_j h_j(x) + \sum_{k=1}^q \mu_k g_k(x)] = max_{\mu} [f(x) + \sum_{k=1}^q \mu_k g_k(x)]$$

而$$\mu_k \ge 0, g_k(x) \le 0 \Rightarrow \mu_k g_k(x) \le 0$$，所以，

$$\theta(x) = f(x)$$

综合以上两点有，

$$\theta(x) = \begin{cases} f(x), \quad \text{x meet constraints} \\  +\infty, \quad other \end{cases}$$

