#### b更新

每次迭代更新了$$\alpha_1, \alpha_2$$ 后，还需要更新$$b$$ 的值，因为上一篇文章中最后那个\(14\)方程组中出现了$$b$$ 这个变量，前面Soft这篇文章的最后分析了根据非边界点（$$0 \lt \alpha_i \lt C$$ ）来更新$$b$$ 值，已知，

$$b = y_i - \sum_{j =1}^m y_j \alpha_j \mathbf x_j \mathbf x_i, \quad \forall i \in [m], 0 \lt \alpha_i \lt C$$

所以，

* 当$$0 \lt \alpha_i \lt C$$，

$$\begin{align} b_1 & = y_1 - y_1 \alpha_1 K_{11} - y_2 \alpha_2 K_{12} - \sum_{j = 3}^m y_j \alpha_j K_{1j} \\ & = y_1 - y_1 \alpha_1 K_{11} - y_2 \alpha_2 K_{12} +  y_1 \alpha_1^{(p)} K_{11} + y_2 \alpha_2^{(p)} K_{12} + b -( \sum_{j=1}^m y_j \alpha_j^{(p)} K_{1j} + b) \\ & = (y_1 - f_1) - y_1 \alpha_1 K_{11} - y_2 \alpha_2 K_{12} +  y_1 \alpha_1^{(p)} K_{11} + y_2 \alpha_2^{(p)} K_{12} + b \\ & = - E_1 - y_1 K_{11} (\alpha_1 - \alpha_1^{(p)}) - y_2 K_{12} (\alpha_2 - \alpha_2^{(p)}) + b \end{align}$$        \(15\)

使用

$$b^{new} = b_1$$ 

来更新

注：

有些文章上面给出的$$b$$ 的更新方程与上面不一样，而是如下方程所示，

$$b_1 = E_1 + y_1 K_{11} (\alpha_1 - \alpha_1^{(p)}) + y_2 K_{22} (\alpha_2 - \alpha_2^{(p)}) + b$$

然而这里说明一下，其实这两者是一致的，因为那些文章中输出函数为$$f(\mathbf x) = \mathbf {wx} - b$$，与我们的输出函数仅$$b$$ 前面的符号相反，如果用$$-b$$ 代替$$b$$ 代入上式，得$$-b_1 = E_1 + y_1 K_{11} (\alpha_1 - \alpha_1^{(p)}) + y_2 K_{22} (\alpha_2 - \alpha_2^{(p)})  - b$$，与\(15\)相同。

*  当$$ 0 \lt \alpha_2 \lt C$$，

同理，

$$b_1 = - E_2 - y_1K_{12}(\alpha_1 - \alpha_1^{(p)}) - y_2 K_{22} (\alpha_2 - \alpha_2^{(p)}) + b$$

使用

$$b^{new} = b_2$$

来更新

*  当$$ 0 \lt \alpha_1, \alpha_2 \lt C$$，

此时，$$b_1 = b_2$$

使用

$$b^{new} = b_1, \quad or \quad b^{new} = b_2$$ 

来更新

*  当$$\alpha_1 , \alpha_2 = 0 \ | \ \alpha_1, \alpha_2 = C $$，并且$$L \neq H$$，

此时$$b_1$$ 和$$b_2$$ 之间所有值都满足KKT条件，都可以作为$$b$$ 的更新值，一般取

$$b^{new} = \frac {\alpha_1 + \alpha_2} 2$$

来更新

####  $$\mathbf w$$ 更新

因为$$\mathbf w = \sum_{i=1}^m y_i \alpha_i \mathbf x_i$$，所以$$\mathbf w$$ 的更新就很直接了，如下，

$$\mathbf w^{new} = \mathbf w + y_1(\alpha_1 - \alpha_1^{(p)}) \mathbf x_1 + y_2 (\alpha_2 - \alpha_2^{(p)}) \mathbf x_2$$







