logistic函数定义如下，

$$\phi_{sig}(z) = \frac 1 {1+exp(-z)}$$                                                                                          \(1\)

于是有，

$$h(\mathbf x) = \phi \circ L_d =  \frac 1 {1 + exp(- \langle \mathbf w, \mathbf x \rangle)}$$                                                                          \(2\)

$$\mathbf w = (b, w_1, ..., w_d)$$

$$\mathbf x = (1, x_1, ...,x_d)$$

\(2\)式取值范围为$$h(\mathbf x) \in (0,1)$$，而二值分类问题的输出为$$y \in \lbrace -1, 1 \rbrace$$，我们希望在$$y=1$$ 的时候$$h(\mathbf x)$$ 比较大，在$$y = -1$$ 的时候$$1- h(\mathbf x)$$比较大，定义二项logistic回归模型是如下条件概率分布，

$$P(Y=1|X=x) = \frac {exp(\langle \mathbf w, \mathbf x \rangle)} {1 + exp(\langle \mathbf w, \mathbf x \rangle)} = h(\mathbf x)$$                                                    \(3\)

$$P(Y=0|X =x) = \frac 1 {1 + exp(\langle \mathbf w, \mathbf x \rangle)} = 1 - h(\mathbf x)$$                                            \(4\)

似然函数为，

$$\prod_{i=1}^m [h(\mathbf x_i)]^{y_i} [1-h(\mathbf x_i)]^{1- y_i}$$                                                                         \(5\)

对数似然函数为，

$$L_{\mathbf w} = \sum_{i=1}^m [y_i(\langle \mathbf w, \mathbf x_i \rangle - log(1+exp(\langle \mathbf w, \mathbf x_i \rangle)))]$$                                     \(6\)

对$$L_{\mathbf w}$$ 求极大值，得到$$\mathbf w$$ 的估计，可以使用梯度下降法。



