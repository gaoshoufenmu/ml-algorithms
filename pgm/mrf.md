马尔可夫随机场（Markov Random Field）

马尔可夫随机场中，多个变量的联合概率能分解为基于最大团的因子的乘积。

设变量为$$\mathbf x = \lbrace x_1, x_2, ..., x_n \rbrace$$，最大团$$\mathbf x_C$$，那么联合概率分布为，

$$P(\mathbf x) = \frac 1 {Z} \prod_C \Psi_C(\mathbf x_C)$$                                                                                                      \(1\)

其中规范化因子

$$Z = \sum_{\mathbf x} \prod_C \Psi_C(\mathbf x_C)$$                                                                                                          \(2\)



