### 学习

已知观测序列和对应的状态序列，则为监督学习，若只有观测序列则为非监督学习。

#### 监督学习

设训练集由$$S$$ 个序列组成，

$$\lbrace (O_1,I_1), (O_2, I_2), ..., (O_S,I_S) \rbrace$$

采用极大似然估计模型参数$$\lambda$$

* 转移概率估计

$$\overline a_{ij} = A_{ij} / (\sum_{j=1}^N A_{ij})$$                                                                                                                               \(1\)

其中，$$A_{ij}$$ 表示训练集中$$i \rightarrow j$$ 状态转移的频数。

* 发射概率估计

$$\overline b_{jk} = B_{jk} / (\sum_{k=1}^M B_{jk})$$                                                                                                                              \(2\)

其中，$$B_{jk}$$ 表示训练集中状态$$j$$ 下观测为$$k$$ 的频数。

* 初始状态概率估计

$$\overline \pi_i = C_i / S$$                                                                                                                                                    \(3\)

其中，$$C_i$$ 表示训练集中初始状态为$$i$$ 的频数。

#### 非监督学习

因为使用了EM算法，所以直接先讨论EM。EM算法用于含有隐变量的概率模型参数的极大似然估计，每次迭代分两步：E，求期望（Expectation）；M，求极大（Maximization）。

##### EM

假设观测样本为$$X = (X_1, X_2, ..., X_S)$$，样本对应的隐变量为$$Y = (Y_1,Y_2, ...,Y_S)$$，模型参数为$$\theta$$，则观测序列的似然函数为，

$$P(X|\theta) = \sum_Y P(X,Y| \theta) = \sum_Y P(Y|\theta) \ P(X|Y, \theta)$$                                                                          \(4\)

可以假设样本之间是独立的，所以，

$$P(X|\theta) = \prod_{X_i} P(X_i|\theta) = \prod_{X_i} \sum_{Y_i} P(X_i,Y_i| \theta) = \prod_{X_i} \sum_{Y_i} P(Y_i|\theta) P(X_i|Y_i, \theta)$$                     \(5\)



现在要求得一个模型参数$$\theta$$，使得似然函数$$P(X|\theta)$$ 达到最大，即，观测序列出现的概率最大，这就是$$\theta$$ 的最大似然估计，\(5\)式中有连乘，通常为了计算方便，将问题转化为求对数似然函数，

$$L(\theta) = \sum_{X_i} \text {log} \sum_{Y_i}  P(X_i,Y_i| \theta) = \sum_{X_i} \text {log} \sum_{Y_i} P(Y_i|\theta) P(X_i|Y_i, \theta)$$                                            \(6\)

因为隐变量$$Y_i$$ 的存在，无法直接求出\(6\)式的极大值，所以通过迭代不断的求$$L(\theta)$$ 的下界（E步），然后优化下界（M步）。

引入一个关于隐变量的函数$$Q_i(Y_i)$$，表示$$Y_i$$ 的某种概率分布，于是，

$$L(\theta) = \sum_{X_i} \text{log} \sum_{Y_i} Q_i(Y_i)[P(X_i,Y_i|\theta) / Q_i(Y_i)]$$

因为在每一步迭代中，可将$$X_i, \theta$$ 看作常量，此时可将$$\sum_{Y_i} Q_i(Y_i)[P(X_i,Y_i|\theta) / Q_i(Y_i)]$$ 看作$$P(X_i,Y_i|\theta) / Q_i(Y_i)$$ 的期望，

由于log对数函数是严格凹函数，根据**Jensen不等式**有（详情可上网搜索，这里不再赘述），

$$E[\text{log} (X)] \le \text{log}(E[X])$$

于是，

$$L(\theta) = \sum_{X_i} \text{log} \sum_{Y_i} Q_i(Y_i)[P(X_i,Y_i|\theta) / Q_i(Y_i)] \ge \sum_{X_i}  \sum_{Y_i} Q_i(Y_i) \text{log} [P(X_i,Y_i|\theta) / Q_i(Y_i)]$$     \(7\)



此时求得似然函数$$L(\theta)$$ 的下界，取决于$$Q_i(Y_i), \ P(X_i,Y_i|\theta)$$这两个值，所以我们（在M步中）通过调整这两个值，优化其下界，**极大化下界来逼近**$$L(\theta)$$** 的极大值**，根据Jensen不等式，当等号成立时，随机变量变成常量值，

$$P(X_i,Y_i|\theta) / Q_i(Y_i) = c$$                                                                                                                                       \(8\)

由于$$Q_i(Y_i)$$ 表示概率分布，所以$$\sum_{Y_i} Q_i(Y_i) = 1$$，从而

$$\sum_{Y_i} P(X_i,Y_i|\theta) = \sum_{Y_i} c \cdot Q_i(Y_i) = c$$                                                                                                            \(9\)

将\(9\)代入\(8\)得，

$$Q_i(Y_i) = P(X_i,Y_i|\theta) / \sum_{Y_i} P(X_i,Y_i|\theta) = P(X_i,Y_i|\theta) / P(X_i|\theta) = P(Y_i|X_i, \theta)$$                                \(10\)

\(10\)式表明$$Q_i(Y_i)$$ 是隐变量$$Y_i$$ 的后验概率。

以上就是E步，接着在M步中，调整$$\theta$$ 来极大化$$L(\theta)$$ 的下界，将下界函数（M步中的目标优化函数）记为，

$$GLB(\theta) = \sum_{X_i}  \sum_{Y_i} Q_i(Y_i) \text{log} [P(X_i,Y_i|\theta) / Q_i(Y_i)]$$

总结算法如下

##### EM算法

$$\text {repeat \{}$$

1. E步：$$\text { foreach i in} [S]$$，计算 $$Q_i(Y_i) := P(Y_i|X_i, \theta_t)$$
2. M步：计算$$\theta_{t+1} := \text{argmax}_{\theta} \sum_{i=1}^S \sum_{Y_i}Q_i(Y_i) \text{log} [P(X_i,Y_i|\theta)/Q_i(Y_i)]$$

$$\}$$

##### 收敛证明

证明$$\theta_{t+1} \ge \theta_t$$。

证：

E步时，$$Q_i(Y_i) = P(Y_i|X_i, \theta_t)$$，因为这是由Jensen等号成立推导出来的，所以此时，

$$L(\theta_t) = \sum_{X_i}  \sum_{Y_i} Q_i(Y_i) \text{log} [P(X_i,Y_i|\theta_t) / Q_i(Y_i)]$$

M步时，固定$$Q_i(Y_i)$$，将$$\theta_t$$ 看作变量，对$$L(\theta_t)$$ 求导并令导数为0，解得$$\theta$$ 作为下一步中的$$\theta_{t+1}$$，此时显然有

$$L(\theta_{t+1}) \ge L(\theta_t)$$

上式在$$\theta_t = \theta_{t+1}$$时等号成立。

停止迭代的条件为：$$|\theta_{t+1} - \theta_t| \lt \epsilon$$  或者$$|GLB(\theta_{t+1}) - GLB(\theta_t)| \lt \epsilon$$

