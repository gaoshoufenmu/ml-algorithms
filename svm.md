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

