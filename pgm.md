概率图模型（Probabilistic Graphical Model）

#### 隐马尔可夫模型

状态变量$$\lbrace y_1, y_2,...,y_n \rbrace$$，$$y_i \in \mathcal Y$$ 表示$$i$$ 时刻的系统状态，状态变量是隐藏的，状态集合$$\mathcal Y = \lbrace s_1,s_2,...,s_N \rbrace$$

观测变量$$\lbrace x_1,x_2,...,x_n \rbrace$$，$$x_i \in \mathcal X$$ 表示$$i$$ 时刻的观测值，观测值集合$$\mathcal X = \lbrace o_1, o_2,...,o_M \rbrace$$

假设：

* 任意时刻，观测值只与当前时刻的系统状态有关

* 任意时刻，系统状态仅与前一时刻的状态有关

这就得到一阶隐马尔可夫模型，如下图，

![](/assets/1_HMM.png)

所有变量的联合概率分布为，

$$P(x_1, y_1,...,x_n,y_n) = P(y_1)P(x_1|y_1) \prod_{i=2}^n P(y_i|y_{i-1}) P(x_i|y_i)$$                                      \(1\)

为了表示方便，增加一个辅助节点$$y_0$$，于是，

$$P(x_1, y_1,...,x_n,y_n) = \prod_{i=1}^n P(y_i|y_{i-1}) P(x_i|y_i)$$                                                                   \(2\)

$$P(y_1|y_0) = P(y_1)$$

根据\(2\)式，要确定一个一阶隐马尔可夫模型，还需要以下三组参数：

##### 参数

\(1\)  状态转移概率矩阵

$$\mathbf A = [a_{ij}]_{N \times N}$$

$$a_{ij} = P(y_{t+1} = s_j|y_t = s_i), \quad 1 \le i,j \le N$$

\(2\) 输出观测概率

$$\mathbf B = [b_{ij}]_{N \times M}$$

$$b_{ij} = P(x_t = o_j|y_t=s_i), \quad 1 \le i \le N, \ 1 \le j \le M$$

\(3\) 初始状态概率

$$\mathbf \pi =[\pi_i]_{1 \times N}$$

$$\pi_i = P(y_1= s_i), \quad 1 \le i \le N$$

以上三组参数记为

$$\lambda = [\mathbf A, \mathbf B, \pi]$$

##### 3个基本问题

1. 概率计算，已知参数$$\lambda$$ 和观测序列$$O$$，计算序列出现的概率$$P(O|\lambda)$$
2. 学习，已知观测序列$$O$$，估计模型参数$$\lambda$$
3. 预测（解码），已知参数$$\lambda$$ 和观测序列$$O$$，求状态序列$$I = argmax_I \ P(I|O)$$ ，即最有可能的状态序列





