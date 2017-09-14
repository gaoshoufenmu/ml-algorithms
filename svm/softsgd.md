在soft文章中我们给出目标优化问题为，

$$min_{\mathbf w, b, \mathbf \xi} \ \frac 1 2 |\mathbf w|^2 + C \sum_{i=1}^m \xi_i, \quad \forall i \in [m], \ y_i(\mathbf {wx}_i + b) \ge 1 - \xi_i, \ \xi_i \ge 0$$

这是由于放宽了约束条件$$y_i(\mathbf {wx}_i + b) \ge 1$$，引入松弛因子$$\xi$$，然而除了这种方法，其实还有很多其他方法，比如我们引入损失函数$$\mathcal l (z)$$，从而将优化问题转为，

$$min_{\mathbf w, b} \ \frac 1 2 |\mathbf w|^2 + C \sum_{i=1}^m \mathcal l (z)$$

使得$$|\mathbf w|$$ 减小的时候，两条边界间距增大，更多的点会越过对应边界（即，$$y_i(\mathbf {wx}_i + b) \lt 1$$），使得损失函数增大，从而第一项和第二项相互制约。

常见的损失函数有，

* 0-1损失函数

$$\mathcal l_{0/1} (z) = \begin{cases} 1, \quad z \lt 0 \\ 0, \quad otherwise \end{cases}$$

此时目标函数为

$$min_{\mathbf w, b} \ \frac 1 2 |\mathbf w|^2 + C \sum_{i=1}^m \mathcal l [y_i(\mathbf {wx}_i + b) - 1]$$

* hinge损失函数

$$\mathcal l_{hingle}(z) = max(0, 1 - z)$$

目标函数为，

$$min_{\mathbf w, b} \ \frac 1 2 |\mathbf w|^2 + C \sum_{i=1}^m max(0, 1- y_i(\mathbf {wx}_i + b))$$

* 指数损失函数

$$\mathcal l_{exp}(z) = exp(-z)$$

目标函数为，

$$min_{\mathbf w, b} \ \frac 1 2 |\mathbf w|^2 + C \sum_{i=1}^m max(0, 1- y_i(\mathbf {wx}_i + b))$$

