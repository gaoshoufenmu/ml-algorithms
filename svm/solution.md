上一篇文章我们知道了求解超平面$$\mathbf {wx} + b = 0$$ 的问题描述

$$argmin_{\mathbf w, b} \ \frac 1 2 |\mathbf w|^2 \quad s.t. \  \forall i \in [m]， y_i(\mathbf {wx}_i + b) \ge 1$$                                                  \(1\)

显然这是一个凸二次规划问题。

利用拉格朗日对偶性，将原问题转化为对偶问题，因为这样求解效率更高。这里又双叒叕给懒人福利了，如果还不了解拉格朗日乘子法，那么可以暂时不用专门去查资料，这里将简单介绍一下（真的很简单地介绍呀喂~）

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

令

$$\theta(x) = max_{\lambda, \mu} \ L(x, \lambda, \mu)$$                                                                                                      \(2\)

对$$\lambda, \mu$$ 求$$L$$ 的最大值，我们要牢牢记住，$$\theta(x)$$ 表示的是每个$$x$$ 取值下的$$L$$ 最大值。于是$$\theta(x)$$ 就只和$$x$$ 有关了，然后我们分析$$x$$ 的取值对函数$$\theta(x)$$ 的影响，

\(1\) 若$$x$$ 不满足原约束条件，即存在 $$h_j(x) \neq 0 \quad or \quad g_k(x)  \gt 0$$，那么，

$$\theta(x) = max_{\lambda, \mu} [f(x) + \sum_{j=1}^p \lambda_j h_j(x) + \sum_{k=1}^q \mu_k g_k(x)] = + \infty$$

这是为什么呢？假设 $$\exists j, h_{j}(x) \neq 0$$，那么当取值 $$\lambda_j \rightarrow \infty$$，且正负性与$$h_j(x)$$ 相同，就有$$\lambda_j h_j(x) \rightarrow +\infty$$，而如果$$\exists k, g_k(x) \gt 0$$，那么当取值 $$\mu_k \rightarrow +\infty$$，就有$$\mu_k g_k(x) \rightarrow +\infty$$，所以此情况下$$\theta(x) = +\infty$$

\(2\) 若$$x$$ 满足原约束条件，此时$$f_j(x) = 0$$，所以，

$$\theta(x) = max_{\lambda, \mu} [f(x) + \sum_{j=1}^p \lambda_j h_j(x) + \sum_{k=1}^q \mu_k g_k(x)] = max_{\mu} [f(x) + \sum_{k=1}^q \mu_k g_k(x)]$$

而$$\mu_k \ge 0, g_k(x) \le 0 \Rightarrow \mu_k g_k(x) \le 0$$，所以，

$$\theta(x) = f(x)$$

综合以上两点有，

$$\theta(x) = \begin{cases} f(x), \quad \text{x meet constraints} \\  +\infty, \quad other \end{cases}$$

现在就很明显了，在$$x$$ 的约束条件下求$$f(x)$$ 的最小值与无约束条件地求$$\theta(x)$$ 的最小值是等价的，所以优化问题转化为求

$$min \quad \theta(x)$$

再考虑到$$\theta(x)$$ 的定义\(2\)式，优化问题就变成了如下，

$$min_{x} \ max_{\lambda, \mu} \ L(x, \lambda, \mu)$$                                                                                               \(3\)

然后再根据拉格朗日对偶性，问题可转化为，

$$max_{\lambda, \mu} \ min_{x} \ L(x, \lambda, \mu)$$                                                                                                \(4\)

而求极值问题可以通过令偏导为0解得。

* 对偶问题

下面分析对偶问题。对偶性可以通过\(3\)和\(4\)式略窥一二，无非就是求最大最小顺序反过来。与上面的$$\theta(x)$$定义类似，我们定义$$\theta_D $$ 函数用以分析本小节的对偶问题，如下，

$$\theta_D (\lambda, \mu) = min_x \ L(x, \lambda, \mu)$$                                                                                         \(5\)

将原始问题中的$$\theta(x)$$ 函数重新记为$$\theta_P (x)$$ 以示区别。

现在，将对偶问题的最优值记为$$d^*$$，

$$d^* = max_{\lambda, \mu} \ \theta_D (\lambda, \mu) = max_{\lambda, \mu} \ min_x \ L(x, \lambda, \mu)$$

将原始问题的最优值记为$$p^*$$，

$$p^* = min_x \ \theta_P (x) = min_{x} \ max_{\lambda, \mu} \ L(x, \lambda, \mu)$$

从函数的定义不难发现有以下关系，

$$\theta_D (\lambda, \mu) = min_x \ L(x, \lambda, \mu) \le L(x, \lambda, \mu)  \le max_{\lambda, \mu} \ L(x, \lambda, \mu) = \theta_P(x)$$

即以下关系成立，

$$\theta_D (\lambda, \mu) \le \theta_P (x), \quad \forall \lambda, \mu, x$$

如果原始问题与对偶问题都有最优值，则

$$max_{\lambda, \mu} \ \theta_D(\lambda, \mu) \le min_x \ \theta_P (x)$$                                                                                 \(6\)

然而我们仅仅得到上式这个关系还不够，还是无法证明对偶问题的最佳解就是原始问题的最佳解。

~~如果\(6\)式严格不等，则称弱对偶性，存在一个对偶间隙\(duality gap\)，如果\(6\)式等式成立，则称强对偶性，此时对偶问题的最佳解与原始问题的最佳解一致。凸函数具有强对偶性，凹函数为弱对偶性，存在对偶间隙。~~

