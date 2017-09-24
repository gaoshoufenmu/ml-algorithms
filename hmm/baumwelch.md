训练集为$$S$$ 个观测序列

$$\lbrace O_1, O_2, ..., O_S \rbrace$$

学习策略是分别对每个观测序列$$O_i$$ 采用Baum\__Welch_计算各自对应的模型参数$$\lambda_i$$，然后分别计算每个模型的状态序列$$I_i$$，然后采用监督学习方法求得模型的参数$$\lambda$$。

某一观测序列为$$O = (o_1,o_2,...,o_n)$$，状态序列为$$I = (i_1,i_2,...,i_n)$$，对数似然函数为$$\text{log}P(O,I|\lambda)$$。根据上一篇文章对EM算法的介绍，E步中有

$$Q(I) := P(I|O, \lambda_t)$$                                                                                                                                         \(1\)

M步中要求极大的函数为，

$$G(\lambda) = \sum_{I} Q(I) \text{log} [P(O,I|\lambda) / Q(I)]$$                                                                                                  \(2\)

与上一篇文章中的稍有不同，原因是这里只对单个观测序列求最大似然估计。

我们容易知道联合概率分布，

$$P(O,I|\lambda) = \pi_{i_1} b_{i_1o_1}a_{i_1i_2}b_{i_2o_2} \cdot \cdot \cdot a_{i_{n-1} i_n}b_{i_n o_n}$$                                                                                                    \(3\)

所以需要将$$G(\lambda)$$ 中的条件概率转化一下，

$$\begin{aligned} G(\lambda) & =\sum_{I}[Q(I) \text{log} P(O,I|\lambda) - Q(I) \text{log} Q(I)] \\ & \rightarrow \sum_{I} Q(I) \text{log} P(O,I|\lambda) \\ & = \sum_{I} \text{log} P(O,I|\lambda) \cdot P(O,I|\lambda_t) / P(O|\lambda_t)  \\ & \propto \sum_{I} \text{log} P(O,I|\lambda) \cdot P(O,I|\lambda_t) \end{aligned}$$                                                                                  \(4\)

第二步中省略了常量$$Q(I) \text{log} Q(I)$$，最后一步省略了常量$$P(O|\lambda_t)$$，因为对求极值不影响。

将\(3\)式代入\(4\)，

$$G(\lambda) = \sum_{I} \text{log} \pi_{i_1} P(O,I|\lambda_t) + \sum_{I}(\sum_{t=1}^{n-1}\text{log} a_{i_ti_{t+1}})P(O,I,|\lambda_t) + \sum_{I}(\sum_{t=1}^n \text{log} b_{i_to_t})P(O,I|\lambda_t)$$    \(5\)

由于$$\lambda=[\pi, \mathbf A, \mathbf B]$$的三个参数分别出现在三项中，所以分别求极大化，

* \(1\) 第一项

$$\sum_{I} \text{log} \pi_{i_1} P(O,I|\lambda_t) = \sum_{i=1}^N \text{log} \pi_i P(O, i_1=i|\lambda_t)$$

左边是对所有状态集合 $$\mathcal I = \lbrace 1,2,...,N \rbrace$$的全排列求和，右边是将这全排列按首个状态的不同分成$$N$$ 组进行求和。

有约束条件

$$\sum_{i=1}^N \pi_i = 1$$                                                                                                                                                    \(6\)

利用拉格朗日乘子法求解。拉格朗日函数为，

$$L(\pi) = \sum_{i=1}^N \text{log} \pi_i P(O, i_1=i|\lambda_t) + \alpha(\sum_{i=1}^N \pi_i-1)$$

对$$\pi_i$$ 求偏导并令其等于0，

$$P(O,i_1=i|\lambda_t) / \pi_i + \alpha = 0, \quad 1 \le i \le N$$                                                                                             \(7\)

求和解得，

$$\sum_{i=1}^N P(O, i_1=i|\lambda_t) + \alpha \sum_{i=1}^N \pi_i = 0 \Rightarrow \alpha = - P(O |\lambda_t)$$

代入\(7\)式，

$$\pi_i = P(O, i_1=i|\lambda_t) / P(O|\lambda_t) = P(i_1 = i|O,\lambda), \quad 1 \le i \le N$$                                                                                        \(8\)

* \(2\) 第二项

$$\sum_{I}(\sum_{t=1}^{n-1}\text{log} a_{i_ti_{t+1}})P(O,I,|\lambda_t) = \sum_{i=1}^N \sum_{j=1}^N \sum_{t=1}^{n-1} \text{log} a_{ij}P(O, i_t=i,i_{t+1} = j|\lambda_t)$$

左边外层求和是对所有状态集合$$\mathcal I = \lbrace 1,2,...,N \rbrace$$的全排列求和，右边则转化为按相邻两个状态的组合分为$$N^2$$ 组求和。

约束条件

$$\sum_{j=1}^N a_{ij} = 1$$

利用拉格朗日乘子法求得

$$a_{ij} = [\sum_{t=1}^{n-1} P(O, i_t=i,i_{t+1}=j|\lambda_t)]/\sum_{t=1}^{n-1} P(O, i_t=i|\lambda_t)$$                                                           \(9\)

* \(3\) 第三项

$$\sum_{I}(\sum_{t=1}^n \text{log} b_{i_to_t})P(O,I|\lambda_t) = \sum_{j=1}^N \sum_{t=1}^n \text{log} b_{jo_t}P(O,i_t=j|\lambda_t)$$

左边外层求和是对所有状态集合$$\mathcal I = \lbrace 1,2,...,N \rbrace$$的全排列求和，右边则是按当前$$t$$ 时刻的状态分成$$N$$ 组求和。

约束条件为

$$\sum_{k=1}^M b_{jk} = 1$$

对三项求极大，将外层求和展开，分别对每个求和项求极大，于是拉格朗日函数为，

$$L_j =\sum_{t=1}^n \text{log} b_{jo_t}P(O,i_t=j|\lambda_t) + \alpha (\sum_{k=1}^M b_{jk} - 1)$$

对$$b_{jk} $$ 求偏导并令其等于0，

$$\sum_{t \in [n], o_t = k} P(O, i_t=j|\lambda_t) / b_{jk} + \alpha = 0, \quad 1 \le k \le M$$                                                                            \(10\)

求和解得，

$$\sum_{k=1}^M \sum_{t \in [n], o_t=k} P(O, i_t = j |\lambda_t) + \alpha \sum_{k=1}^M b_{jk} = 0 \Rightarrow \alpha = - \sum_{t=1}^n P(O, i_t=j|\lambda_t)$$

代入\(10\)式得，

$$b_{jk} = \sum_{t=1}^n P(O, i_t= j|\lambda_t)I(o_t= k) / \sum_{t=1}^n P(O, i_t=j|\lambda_t)$$                                                                  \(11\)

虽然公式推导已经给出来了\(8\),\(9\)和\(11\)，但是实际如何操作与计算？

对于\(8\)式，分母$$P(O|\lambda_t)$$ 可以采用本章第一篇文章中的前向向算法计算得到，首先给出相关的公式，

$$\gamma_t(i) = P(i|O, \lambda) = \alpha_t(i) \beta_t(i) / \sum_{i=1}^N \alpha_t(i) \beta_t(i)$$                                                                                      \(12\)

$$\xi_t(i,j) = P(i, j|O, \lambda) = \alpha_t(i) a_{i j} b_{j o_{t+1}} \beta_{t+1}(j) / \sum_{i=1}^N \sum_{j=1}^N \alpha_t(i) a_{ij} b_{jo_{t+1}} \beta_{t+1}(j)$$                             \(13\)

于是模型参数变为，

$$\pi_i = \gamma_1(i)$$                                                                                                                                                               \(14\)

$$a_{ij}=\sum_{t=1}^{n-1} \xi_t(i,j) / \sum_{t=1}^{n-1} \gamma_t(i)$$                                                                                                                       \(15\)

$$b_{jk} = \sum_{t=1}^n \gamma_t(j) I(o_t=k) / \sum_{t=1}^n \gamma_t(j)$$                                                                                                         \(16\)

