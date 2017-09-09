对二分类问题，假设有一组样本，如下图，圆圈表示负例，方块表示正例，图中给出了几种划分超平面，显然还有很多很多划分超平面，但是直觉告诉我们绿色的那个划分超平面最好，因为其泛化能力最好，即使训练样本局部有扰动也能容忍，那么如何使用数学语言来描述呢？

![](/assets/svm_sp.png)

想想我们的目的是什么，要找到一个划分超平面，使得样本中与超平面最近的点与超平面的距离尽可能的大，所以数学语言描述显然为：

$$argmax_{\mathbf w, b} \quad min_{i \in [m]}D_i$$

其中，$$\mathbf w, b$$ 是超平面的参数，$$D_i$$ 表示$$m$$ 个样本中各样本点与超平面的距离

使用如下线性方程描述超平面，

$$\mathbf {wx} + b = 0$$

超平面的法向量为$$\mathbf w = (w_1,w_2,...,w_d)$$

这里稍微解释一下，看上图棕色的两个平行线，显然其直线方向向量和法向量对应分别相同（平行线性质），由棕色的虚线，不难知道其方向向量为$$(1,k)$$（因为点$$(1,k)$$在这条线上嘛），而法向量与方向向量垂直，即向量相乘值为0，所以不难知道$$(-k,1)$$是其法向量（因为$$-k \cdot 1 + 1 \cdot k = 0$$），而直线$$x_2=kx_1 \Rightarrow -kx_1+x_2 = 0 \Rightarrow \mathbf {wx} = 0$$，到这里，解释完毕 -\_-

按剧情发展，现在应该是要研究一下距离了吧？

讲道理，这是初中几何题了，如果忘记了也无妨，再看一下课本就知道了。嘛，作为给懒人看的文章，自然不会要求懒人去翻课本，这里简单说下（根本没法复杂了阿鲁~），看下图

![](/assets/distance.png)

点$$P$$ 垂直于直线于点$$Q$$，于是，向量$$\vec {PQ} || \vec n$$，，令$$\vec {PQ} = \lambda \vec n$$，即，$$\mathbf x_Q - \mathbf x_P = \lambda \vec n$$，根据上面的介绍，法向量$$\vec n$$ 取$$\mathbf w$$，于是

$$\mathbf x_Q = \mathbf x_P + \lambda \mathbf w$$

由于点$$Q$$位于直线上，所以$$\mathbf w (\mathbf x_P + \lambda \mathbf w) + b = 0$$，于是有

$$\lambda = - \frac {\mathbf w \mathbf x_P + b} {\mathbf w^2}$$

所以距离为 $$|\vec {PQ}| = |\lambda||\mathbf w| = \frac {|\mathbf w \mathbf x_P + b|} {|\mathbf w|^2} |\mathbf w| = \frac {|\mathbf w \mathbf x_P + b|} {|\mathbf w|}$$

结论: 点$$\mathbf x$$ 与直线$$\mathbf {wx} + b = 0$$ 的距离

$$d = \frac {|\mathbf {wx} + b|}{|\mathbf w|}$$

可以令$$|\mathbf w| = 1$$，等价于将直线$$\mathbf {wx} + b = 0$$ 两边同除以$$|\mathbf w|$$，毫无影响的说，直线还是那条直线，于是距离的形式为

$$d = |\mathbf {wx} + b|$$

眼看这一页也洋\(啰\)洋\(啰\)洒\(嗦\)洒\(嗦\)写了好多内容，可是与SVM直接相关的好像并没有多少（啊~痛苦脸），咳咳，写东西喜欢旁征博引不是坏事（故作安慰自我状，明明就是没抓住主题好嘛）。

赶紧回归一波主题~

我们不要忘记了找最佳的划分超平面时有一个前提，就是所有样本点分类正确，看本页第一个图，我们令（这是“你们”令的，我只是遵循这一约定，摊手~ 当然反过来也不是不可，只是没必要特立独行，毕竟殊途最终还是同归）正例（$$y=1$$）的样本点满足$$\mathbf {wx} + b \gt 0$$，负例（$$y=-1$$）的样本点满足$$\mathbf {wx} + b \lt 0$$ ，于是对所有$$m$$ 个样本点均有，

$$\forall i \in [m], \quad y_i(\mathbf {wx_i} + b) > 0$$

综上，我们要找的划分超平面的数学语言描述为，

$$argmax_{(\mathbf w, b): |\mathbf w| = 1} \quad min_{i \in [m]} |\mathbf {w x}_i + b| \quad s.t. \quad \forall i, y_i(\mathbf {wx}_i + b) > 0$$              \(1\)

接下来，我们相对上式进行一些改写，或者说是变形，也许变形后实际中求解更方便呢（美滋滋~）

观察上式，可以发现$$|\mathbf {wx}_i + b| = |y_i(\mathbf {wx}_i + b)| = y_i(\mathbf {wx}_i + b) \gt 0$$

$$|\mathbf {wx}_i + b| = |y_i(\mathbf {wx}_i + b)| = y_i(\mathbf {wx}_i + b) \gt 0$$                                                      \(1.5\)

所以\(1\)式可以改写为，

$$argmax_{(\mathbf w, b): |\mathbf w| = 1} \quad min_{i \in [m]} y_i(\mathbf {w x}_i + b)$$                                                                 \(2\)

不用担心上式中$$y_i(\mathbf {wx}_i+b) \le 0$$，因为在外层，我们还要求解使其最大，从而能够保证$$y_i(\mathbf {wx}_i+b)\gt 0$$，只要样本是线性可分的（对于线性不可分的情况，后面再介绍）。

再观察上面两式，发现又要求最小又要求最大的，甚是麻烦，想办法统一一下，要么只求最小，要么只求最大，这样才好使用线性规划的方法来求解。

————————————&gt; 前方高能 ————————————&gt; 前方高能 ————————————&gt;

假设已经求得\(1\)式的解为$$(\mathbf w^*, b^*)$$，那么$$|\mathbf w^*|=1$$，那么最靠近超平面的样本点与超平面距离记为$$\gamma^* = min_{i \in [m]} y_i(\mathbf {wx}_i + b)$$，那么所有点与超平面的距离均大于等于这个值，根据\(1.5\)式，距离可用$$y_i(\mathbf {wx}_i + b)$$表示，于是，

$$y_i(\mathbf {w^*x}_i + b^*) \ge \gamma^*$$

最小距离大于0，变换一下得，

$$y_i(\mathbf {\frac {w^*} {\gamma^*} x}_i + \frac {b^*} {\gamma^*}) \ge 1, \quad \forall i \in [m]$$                                                                                       \(3\)

上式表示的是限制条件。

既然$$(\mathbf w^*, b^*)$$表示我们要找的超平面，那$$(\frac {\mathbf w^*}{\gamma^*}, \frac {b^*}{\gamma^*})$$ 仍然表示那个超平面，只不过超平面参数没有归一化嘛，又没有什么影响是吧，我们再用一般化的符号$$(\mathbf w, b)$$ 来表示超平面，于是

$$\mathbf w = \frac {\mathbf w^*}{\gamma^*} \Rightarrow \gamma^* = \frac {|\mathbf w^* |} {|\mathbf w|} = \frac 1 {|\mathbf w|}$$                                                                                              \(4\)

根据\(1\)式中的目标函数是求$$\gamma^*$$的极大值，根据\(4\)式就是求$$\mathbf w$$ 的极小值，再结合\(3\)式所代表的限制条件，问题转化为，

$$argmin_{\mathbf w, b} \frac 1 2 |\mathbf w|^2 \quad s.t. \forall i \in [m], y_i(\mathbf {wx}_i + b) \ge 1$$                                                      \(5\)

上式，就是支持向量机SVM的基本型，嗯，一点都不高能~

