### 从逻辑回归开始
逻辑回归的模型形式：$$h_{\theta}(x)=\frac{1}{1+e^{-\theta^Tx}}$$令$z=\theta^Tx$，那么就有$$h_{\theta}(x)=\frac{1}{1+e^{-z}}$$如果我们希望$\hat{y}=1$那么就意味着$z \gg 0$；反之就意味着$z \ll 0$。
其代价函数为：$$Cost(x)=-(y\log(h_{\theta}(x))+(1-y)\log(1-h_{\theta}(x)))$$对于支持向量机，我们需要在这个代价函数上做一些小修改，使得它对于传统的逻辑回归具有计算上的优势。
**逻辑回归的优化目标：**$$min_{\theta}\frac{1}{m}\left[ \sum_{i=1}^m y^{(i)}(-\log(h_{\theta}(x^{(i)})))+(1-y^{(i)})(-\log(1-h_{\theta}(x^{(i)})))+\frac{\lambda}{2m}\sum_{j=1}^n\theta_{j}^2 \right]$$**SVM的优化目标：**$$min_{\theta}\frac{1}{m} \sum_{i=1}^m \left[y^{(i)}(cost_{1}(z))+(1-y^{(i)})(cost_{0}(z))\right]+\frac{\lambda}{2m}\sum_{j=1}^n\theta_{j}^2$$为了简化计算，我们在这里去除$\frac{1}{m}$项(并不会影响结果)，并令$C=\frac{1}{\lambda}$，那么就得到：$$min_{\theta}~ C~\sum_{i=1}^m \left[y^{(i)}(cost_{1}(z))+(1-y^{(i)})(cost_{0}(z))\right]+\frac{1}{2}\sum_{j=1}^n\theta_{j}^2 $$
##### 支持向量机要输出什么
支持向量机会直接作出0或1的判断，如下：$$h_{\theta}(x)=\left\{\begin{array} \\
1~~~~~~~~~~~if~~\theta^Tx \geq 0 \\
0~~~~~~~~~~~~~otherwise
\end{array}\right.$$

### 大间距分类器
SVM也被称为大间距分类器，它的两个$cost$函数图像如下所示：
![[Pasted image 20241229214600.png]]
其中
- 当$y=1$，我们希望$\theta^Tx\geq 1$，而不只是$\geq 0$
- 当$y=0$，我们希望$\theta^Tx \leq -1$，而不只是$\leq 0$
那么，如果我们的优化目标即为，选择参数矢量$\theta$，使得$$\sum_{i=1}^m \left[y^{(i)}(cost_{1}(z))+(1-y^{(i)})(cost_{0}(z))\right]$$项为0，那么这个时候，唯一的优化项即为：$$\frac{1}{2}\sum_{j=1}^n\theta_{j}^2$$这意味着，我们找出一条线，使得它距离两个类都尽可能远：
![[Pasted image 20241229215558.png]]
比如图中的黑色线(决策边界)。这时候，决策边界距离两个类的边界(蓝色线)的距离就是这个SVM的margin(边缘)。
$C$往往**不能被设置得太大**。否则，若数据集中存在少量异常样本或者数据集线性不可分时，SVM就不能很好地工作。

### SVM的原理
**内积与支持向量：**
![[Pasted image 20241230152137.png]]
对于任意维度空间中的矢量，我们有：$\vec{v}\cdot \vec{u}=p\cdot ||u||$
其中：
- $p$为$\vec{v}$在$\vec{u}$上投影的模长(如果$\vec{v}$和$\vec{u}$的夹角大于90°，那么$p$的值就小于0)
- $||u||$为矢量$u$的$L_{2}$范数。

假设有一个SVM，其具有两个特征$(x_{1},x_{2})$，且$\theta_{0}=0$，那么对于它的完美分类损失函数即为：$$min_{\theta}\frac{1}{2}\sum_{j=1}^n\theta_{j}^2=\frac{1}{2}(\sqrt{ \theta_{1}^2+\theta_{2}^2 })^2=\frac{1}{2}||\theta||^2$$对于任意一个样本点$(x^{(i)}_{1},x^{(i)}_{2})$，作出其矢量$\vec{x}^{(i)}$，那么我们就可以知道$\theta^T\cdot x$为如下图，这时候$\theta^T\cdot x$必然大于1(或小于-1)。
![[Pasted image 20241230153404.png]]
假设现在有这样的一组样本点：
![[Pasted image 20241230153710.png]]
如果SVM选择了这样一个决策边界：
![[Pasted image 20241230153732.png]]
这个时候我们去计算各个样本的$p^{(i)}\cdot||\theta||$，因为$p^{(i)}$都非常小(品红色线的长度)，这时候我们就必然需要$||\theta||$非常大，但同时，我们的优化函数又强迫我们的$||\theta||$尽可能小，这两个目标相冲突，因此上述决策边界不是一个好的决策边界。
![[Pasted image 20241230153945.png]]
而对于这样的一个决策边界，$p^{(i)}\cdot||\theta||$中$p^{(i)}$就大得多了，因此也可以获得一个更小的$||\theta||$，因此SVM更倾向于使用这样的决策边界。

## 核函数(Kernels)
在SVM中，我们使用核函数来指代我们对特征的不同高阶组合。
比如在传统的线性回归中，我们使用
	$\theta_{0}+\theta_{1}x_{1}+\theta_{2}x_{2}+\dots+\theta x_{n}+\theta_{n+1}x_{1}^2+\theta_{n+2}x_{2}^2$
来描述我们的回归方程，这时候高次项就会有很多不同的组合，比如$x_{1}^2$，$x_{2}^2$，$x_{2}x_{2}$等等。
那么这时候就可以使用一种思路，即使用一定的$f$来作为一个模板函数，来界定我们选择什么样的特征组合来进行SVM计算，这个模板函数$f$就是核函数。如下例子：
	$\theta_{0}+\theta_{1}f_{1}+\theta_{2}f_{2}+\dots+\theta_{n}f_{n}$

#### 例子
比如在空间中有三个点$l_{1},l_{2},l_{3}$，现在我要获得3个新的高阶组合$f_{i}$，并对$f_{i}$做如下度量：$$\begin{array} \\
f_{1}=similarity(x,l^{(1)})=\exp\left( -\frac{||x-l^{(1)}||^2}{2\sigma^2} \right) \\
f_{2}=similarity(x,l^{(2)})=\exp\left( -\frac{||x-l^{(2)}||^2}{2\sigma^2} \right) \\
f_{3}=similarity(x,l^{(3)})=\exp\left( -\frac{||x-l^{(3)}||^2}{2\sigma^2} \right) \\
\end{array}$$其中，$similarity(x,l^{(i)})$就被称为核函数，其中$\exp$被称为高斯核函数。核函数一般被记为$k(x,l^{(i)})$。
对于上述的高斯核函数，假设$x\approx l^{(i)}$：
	那么，就有$f_{i}\approx \exp\left( -\frac{0^2}{2\sigma^2} \right)\approx 1$
假设$x$距离$l^{(i)}$非常远，那么就有：
	$f_{i}\approx \exp\left( -\frac{(large~number)^2}{2\sigma^2} \right)\approx 0$
这个函数就能衡量给定点$x$和点$l^{(i)}$之间的相似度$similarity$。
![[Pasted image 20241230155922.png]]
以上是该相似度函数(假设点为(5, 5))的图像。
假设三个点的图像如下图所示：
![[Pasted image 20241230161526.png]]
假设我们有一个模型，当且仅当$$\theta_{0}+\theta_{1}f_{1}+\theta_{2}f_{2}+\theta_{3}f_{3}\geq 0$$判断值为1，其中$\theta_{0}=0~~,~~\theta_{1}=\theta_{2}=\theta_{3}=1$
那么我们这个模型就可以判断所选点和$l^{(1)},l^{(2)},l^{(3)}$之间的相似性，当且仅当某个点$x$非常接近这三个点中的至少其中一个时，模型输出才会为1。

如果使得$\theta_{3}=0$，即忽视点$l^{(3)}$，那么最终的决策边界大概像这样：
![[Pasted image 20241230161555.png]]
是一个围绕着点$l^{(1)},l^{(2)}$的边界。

### 有核SVM
给定点集$$(x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}),\dots,(x^{(m)},y^{(m)})$$那么，就选择$l^{(1)}=x^{(1)},l^{(2)}=x^{(2)},\dots,l^{(n)}=x^{(n)}$。
对于任意一个训练样本$x$，有：$$\begin{array}\\
f_{1}=sim(x,l^{(1)}) \\
f_{2}=sim(x,l^{(2)}) \\
\dots
\end{array}$$对于整个训练样本集$(x^{(i)},y^{(i)})$：$$x^{(i)} \to \left\{\begin{array} \\
f_{1}^{(i)}=sim(x^{(i)},l^{(1)}) \\
f_{2}^{(i)}=sim(x^{(i)},l^{(2)}) \\
\dots \\
f_{m}^{(i)}=sim(x^{(i)},l^{(m)})
\end{array}\right.$$将这些数据合并为总的特征矢量$f$，$$f=\left[\begin{array}\\
f_{0} \\
f_{1} \\
f_{2} \\
\dots \\
f_{n}
\end{array}\right]$$其中$f_{0}=1$，SVM要做的就是当$\theta^Tf\geq 0$时，输出为1。
此时的$J$代价函数为：$$J(\theta)=C\sum_{i=1}^m [y^{(i)}cost_{1}(\theta^Tf^{(i)})+(1-y^{(i)})cost_{0}(\theta^Tf^{(i)})] + \frac{1}{2}\sum_{j=1}^n \theta_{j}^2$$其中$C=\frac{1}{\lambda}$，优化目标即为$min_{\theta}J(\theta)$，注意，此处依然不会正则化$\theta_{0}$。如果我们直接使用数据集中的点来作为类中心，这时候就有$n=m$。
$\sum_{j}\theta_{j}^2$也可以写成$\theta^T\theta$，对于有的算法，取决于我们使用了哪种核函数，有时候也会加上一个矩阵$M$，即$\theta^T M \theta$。

**对于SVM，更大的$C$往往导向高方差，低误差；更小的$C$往往导向高误差，低方差。**
对于相似性函数$sim(x,l)$，更小的$\sigma^2$往往意味着函数图像越陡，反之则函数图像越平缓。
![[Pasted image 20241230164509.png]]

## 使用SVM
使用SVM要指定两个值：
- 参数$C$
- 使用什么样的核。

#### 核函数选择
1) 线性核(即无核)
	当$\theta^Tx>0$，判断$y=1$。当特征数很大而样本数较少时，判定一个线性决策边界是比较合理的。
2) 高斯核(sim函数)
	需要考虑选择怎么样的$\sigma^2$。当我们的特征比较少，而训练样本数比较大时，我们想要一个比较复杂精确的非线性核函数。
3) 多项式核
	$k(x,l)=(x^Tl+b)^d$，需要选择$b$和$d$。很少用，在通常情况下效果都比较差，它通常只用在所有数据都严格非负的时候，那么它们的差距会被指数拉大。
4) 串核、卡方核等。

#### 关于多分类
如果要进行一个类数量大于2的多分类任务，我们可以训练多个SVM模型，每一个模型执行一个二分类任务，将输出的判别矢量作为最终的判别值。

### 在SVM和逻辑回归之间选择
给定$n$为特征的数量($x \in \mathbb{R}^{n+1}$)，$m$为训练样本的数量。
- 如果$n$相对于$m$非常大(比如10000个特征对10-1000个样本)，那么就使用逻辑回归或者无核的SVM。
- 如果$n$比较小(不大于1000)，而$m$大小适中(10到50000)，使用高斯核的SVM。
- 如果$n$比较小(不大于1000)，而$m$非常大(大于50000)，那么使用逻辑回归或无核的SVM。