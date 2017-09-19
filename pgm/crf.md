最大熵原理：给定一个**概率分布不完全**的信息，对于未知部分的信息，总是认为是均匀分布的。

条件熵定义为，

$$H(y|x) = - \sum_{x \times y \in \mathcal Z} p(x,y) \ \text {log} \  p(y|x)$$                                                                                    \(1\)

$$\mathcal X$$ ： 输入空间

$$\mathcal Y$$ ： 输出空间

$$\mathcal Z$$ ： 样本空间

##### ME

最大熵模型是一个条件概率分布$$P(Y|X), \ X \in \mathcal X \subset \mathbf R^n, \ Y \in \mathcal Y$$。

给定训练集

$$D = \lbrace (\mathbf x_1, y_1), ..., (\mathbf x_N, y_N) \rbrace$$

使用最大熵原理学习得到最好的分类模型，即，要在符合训练集的所有模型集合$$\mathcal P$$ 中找到一个条件概率分布$$p^*(y|\mathbf x)$$，使得条件熵达到最大，

$$p^*(y|\mathbf x) = \text {argmax}_{p(y|\mathbf x) \in \mathcal P} \ H(y|\mathbf x) = \text {argmax}_{p(y|\mathbf x) \in \mathcal P} \ - \sum_{\mathbf x \times y \in \mathcal Z} p(x,y) \ \text {log} \  p(y|\mathbf x)$$     \(2\)

输入$$\mathbf x$$ 和输出$$y$$ 之间一定是存在某些约束关系的（否则，就不用求解模型了，直接样本空间$$\mathcal Z = \mathcal X \times \mathcal Y$$ 等可能的出现样本点，这符合最大熵原理），用一组特征函数$$\lbrace f_i(\mathbf  x, y), \ 1 \le i \le m \rbrace$$ 描述这些约束关系。简单起见，我们使用二值函数，

$$f_i(\mathbf x, y) = \begin{cases} 1 \quad \mathbf x \ \text {and} \ y \ \text {meets some relation} \\ 0 \quad otherwise \end{cases} $$                                                                         \(3\)

然而，光有上式这种形式的约束条件还不够，因为我们要求\(2\)式目标函数的优化问题，目标函数是各种概率分布，所以势必需要将约束条件与概率分布联系起来。

给出\(2\)式时我们提到，要在符合训练集的模型中寻找合适的条件概率分布$$p^*(y|\mathbf x)$$（毕竟，训练集是我们的已知信息），那么，每个特征函数$$f_i(\mathbf x, y)$$ 经验期望与基于$$p^*(y|\mathbf x)$$ 的期望两者相等，

$$E(f_i) = \overline E (f_i)$$                                                                                                                                     \(4\)

而特征的两种期望为，

$$\overline E (f_i) = \sum_{\mathcal Z} \overline p(\mathbf x, y) f_i(\mathbf x,y)$$                                                                                                            \(5\)

$$E(f_i) = \sum_{\mathcal Z} p(\mathbf x,y) f_i(\mathbf x, y) = \sum_{\mathcal Z} p(\mathbf x) p(y|\mathbf x) f_i(\mathbf x, y) \approx \sum_{\mathcal Z} \overline p(\mathbf x) p(y|\mathbf x) f_i(\mathbf x, y)$$         \(6\)

\(6\)式最后一步近似是因为没法知道真正的$$p(\mathbf x)$$，所以只好用经验概率分布$$\overline p(\mathbf x)$$ 代替，这样就出现了我们要求的$$p(y|\mathbf x)$$ 这一因子。

给定训练集，可以得到经验联合概率分布$$\overline P(X,Y)$$ 和经验边缘概率分布$$\overline P(X)$$（频率除以数据集大小），

