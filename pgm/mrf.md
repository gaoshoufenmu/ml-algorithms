#### 马尔可夫随机场（Markov Random Field）

马尔可夫随机场中，多个变量的联合概率能分解为基于最大团的因子的乘积。

设变量为$$\mathbf x = \lbrace x_1, x_2, ..., x_n \rbrace$$，最大团$$\mathbf x_C$$，那么联合概率分布为，

$$P(\mathbf x) = \frac 1 {Z} \prod_C \Psi_C(\mathbf x_C)$$                                                                                                      \(1\)

其中规范化因子

$$Z = \sum_{\mathbf x} \prod_C \Psi_C(\mathbf x_C)$$                                                                                                          \(2\)

#### 条件随机场（CRF）

我们先看下图中几个概率模型的关系，借助其中的几个概率模型来理解CRF。

![](/assets/CRF_model.png)

##### Naive Bayes

给定一输入向量$$\mathbf x$$，判断其分类$$y$$，使用条件概率

$$p(y|\mathbf x) = \frac {p(\mathbf x, y)} {p(\mathbf x)} \propto p(\mathbf x, y)$$                                                                                                                         \(3\)

从\(3\)式中看出给定输入向量$$\mathbf x$$ 的情况下，$$p(y|\mathbf x) \propto p(\mathbf x, y)$$，所以求解其对应分类转化为求解使得**联合概率**最大的那个分类，

$$y = argmax_y \ p(\mathbf x, y)$$

然而，这个联合概率计算可能会比较复杂，尤其在$$\mathbf x = (x_1,...,x_m)$$ 的$$m$$ 很大的情况，

$$p(\mathbf x, y) = p(y) p(\mathbf x| y) = p(y) p(x_1|y) \prod_{i=2}^m p(x_i|x_{i-1},...,x_1,y)$$

所以，为了简单，我们假设$$\mathbf x$$ 的各分量之间相互独立，这就是**朴素贝叶斯假设**，于是，

$$p(y|\mathbf x) \propto p(\mathbf x, y) = p(y) \prod_{i=1}^m p(x_i|y)$$                                                                                                   \(4\)

##### HMM

从单个分类预测扩展到序列分类预测，就是NB向HMM转换。

假设长度为$$n$$ 的观测序列为$$\vec x = \lbrace x_1,x_2,...,x_n \rbrace$$，预测状态序列$$\vec y = \lbrace y_1,y_2,...,y_n \rbrace$$，$$i$$ 时刻观测值$$x_i$$ 对应的状态为$$y_i$$，如果将任一时刻$$i$$ 看作一个NB模型，那么联合概率为，

$$p(x_i,y_i) = p(y_i)p(x_i|y_i)$$

于是，**将**$$n$$** 个 NB模型连接起来**，联合概率为，

$$p(\vec x, \vec y) = \prod_{i=1}^n p(y_i)p(x_i|y_i)$$

上面这个式子的独立性假设比较强，要求各时刻之间的状态相互独立，每个时刻$$i$$ 的观测值$$x_i$$ 仅依赖当前的状态$$y_i$$，因此，可以适当放宽假设条件，假设时刻$$i$$ 的状态$$y_i$$ 依赖于上一时刻$$y_{i-1}$$，于是联合概率为，

$$p(\vec x, \vec y) = \prod_{i=1}^n p(y_i|y_{i-1})p(x_i|y_i)$$                                                                                                              \(5\)

$$p(y_1|y_0) = p(y_1)$$                                                                                                                                              \(6\)

其中$$y_0$$ 的引入是为了书写统一。

**结论**：使得\(5\)式联合概率最大的状态序列就是要求的状态序列。

计算观测序列$$\vec x$$ 出现（所有可能的状态序列）的概率则使用下式，

$$p(\vec x) = \sum_{\vec y \in \mathcal Y^n} \prod_{i=1}^n p(y_i|y_{i-1})p(x_i|y_i)$$                                                                                                      \(7\)

