上一篇文章“大肆”介绍了拉格朗日对偶，耗费了不少篇幅，以致都快忘记我们的本来目的了，在这篇文章中赶紧回归正题，我们现在要计算

$$argmin_{\mathbf w, b} \ \frac 1 2 |\mathbf w|^2 \quad s.t. \  \forall i \in [m]， y_i(\mathbf {wx}_i + b) \ge 1$$                                                                       \(\*\)

拉格朗日函数为，

$$L(\mathbf w, b, \mathbf{\alpha}) = \frac 1 2 |\mathbf w|^2 + \sum_{i=1}^m \mathbf {\alpha_i} (1 - y_i (\mathbf {wx}_i + b)), \quad \mathbf {\alpha} = (\alpha_1, ..., \alpha_m)$$                                     \(1\)

利用拉格朗日对偶性，我们转为去求解

$$max_{\alpha} \ min_{\mathbf w, b} \ L$$

#### 求解$$\mathbf w$$ 值

对$$\mathbf w, b$$ 求偏导并令其等于0（分别对$$\mathbf w$$ 的各个分量求偏导），

$$\nabla_{w^j} L = w^j - \sum_{i=1}^m \alpha_i y_i \mathbf x_{i}^j = 0$$

$$\nabla_b L = - \sum_{i=1}^m \alpha_i y_i = 0$$

注意，上两式中，$$w^j$$ 是向量$$\mathbf w$$ （维度记为$$d$$）的第$$j$$ 个分量，$$(\mathbf x_i, y_i)$$是$$m$$ 个样本中第$$i$$ 个样本，$$\mathbf x_{i}^j$$ 表示样本点$$\mathbf x_i$$ 的第$$j$$ 维分量，所以，

$$\mathbf w = \sum_{i=1}^m \alpha_i y_i \mathbf x_i = 0$$                                                                                      \(2\)

$$\sum_{i=1}^m \alpha_i y_i = 0$$                                                                                                    \(3\)

\(2\)代入\(1\)式并化简得对偶问题并注意到\(3\)这个约束条件，于是问题转化为，

$$max_{\alpha} - \frac 1 2 \sum_{i=1}^m \sum_{k=1}^m \alpha_i \alpha_k y_i y_k \mathbf x_i \mathbf x_k + \sum_{i=1}^m \alpha_i$$                                   \(4\)

$$\sum_{i=1}^m \alpha_i y_i  = 0$$

$$\alpha_i \ge 0, \quad i = 1,2,...,m$$

我们改写一下约束条件以便于分析，如下

$$\begin{cases} \alpha_i \ge 0 \\ y_i f(\mathbf x_i) - 1 \ge 0 \\ \alpha_i (y_i f(\mathbf x_i) - 1) = 0 \end{cases}$$

其中第二项成立的理由是，根据\(\*\)式中的约束条件有，

$$y_i(\mathbf {wx}_i + b) \ge 1 \Rightarrow y_i f(\mathbf x_i) - 1 \ge 0$$

另外\(\*\)式有最优解需要满足KKT（Karush-Kuhn-Tucher）条件（参见上一篇文章中关于KKT条件的第四条强对偶性条件），于是有第三项，

$$\alpha_i(1- y_i(\mathbf {wx}_i + b)) = 0$$

即对任意样本点，总会满足$$\alpha_i = 0 $$ 或 $$f(\mathbf x_i) = 1$$，根据\(2\)式，我们知道最优解$$\mathbf w$$ 只与$$ \alpha_i > 0$$ 的点有关，此时必然有$$f(\mathbf x_i) = 1$$，也就是说，这些样本点位于最大间隔边界上，是支持向量$$(\mathbf x_i, y_i)$$，这也是支持向量机SVM的本质意义，这些点决定了间隔边界的唯一以及分离超平面的唯一。

#### 求解b值

对所有样本点，必然$$\exists i \in [m], \alpha_i > 0$$，否则如果$$\forall i \in [m], \alpha_i = 0$$，那么根据\(2\)式可知$$\mathbf w = \mathbf 0$$，显然不是我们所要的最优解。那么，假设$$j \in [m], \alpha_j > 0$$，于是$$f(\mathbf x_j) =y_j(\mathbf {wx}_j + b)= 1$$，两边同乘以$$y_j$$，将\(2\)式代入解得，

$$b = y_j -  \sum_{i=1}^m \alpha_i y_i(\mathbf x_i \mathbf x_j)$$，   其中找的一个样本点$$(\mathbf x_j , y_j)$$满足，$$j \in [m], \alpha_j > 0$$

实际计算中采用一种更好的办法，取平均值，设$$S = \lbrace i | \alpha_i > 0, i \in [m] \rbrace$$ 为所有支持向量的下标集，那么，

$$b = \frac 1 {|S|} \sum_{s \in S} [y_s - \sum_{i=1}^m \alpha_i y_i (\mathbf x_i, \mathbf x_j)] = \frac 1 {|S|} \sum_{s \in S} [y_s - \sum_{s \in S} \alpha_i y_i (\mathbf x_i, \mathbf x_s)] $$

第二个等式成立的原因前面也分析过了，$$\mathbf w$$的值只跟支持向量有关。

#### SMO

求对偶问题\(4\)的最佳解$$\alpha^*$$可以使用动态规划，然而在样本点很多的情况下复杂度会变得很高，于是有高人就研究出了一个更高效的算法Sequential Minimal Optimization \(SMO\)。

SMO的主要思路是将\(4\)式这个二次规划问题分成一系列的很小的二次规划问题。

SMO每次选取两个变量$$\alpha_i, \alpha_j$$，并固定其他参数，选择两个变量的原因是因为约束\(3\)式，选择一个变量，而固定其他$$m-1$$变量的话，那这个变量本身已经可以被计算出来了。好了，选择了两个变量$$\alpha_i , \alpha_j$$后，求解\(4\)式的最优解，

两个变量$$\alpha_i , \alpha_j$$上的优化问题可以有解析解，避免了数值解，从而计算效率大大提升。于是我们将SMO分成两个部分讨论：1）两个拉格朗日乘子变量上的优化问题的解析解；2）启发式地决定选择哪两个拉格朗日乘子。

