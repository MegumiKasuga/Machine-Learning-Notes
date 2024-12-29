### 分类问题
- 分类问题指的是输出值有且仅有0和1(二值)的一类问题。
- 也可以把0类称为负类，1类称为正类。
- 线性回归算法对于分类问题往往是不合适的。

### 逻辑回归
$Logistic~Regression$是一种分类算法，它的假设函数$h$的输出值保持在$[0, 1]$之间。

#### $h$函数
$$h_{\theta}(x)=g(\theta^Tx)~~~,~~~g(z)=\frac{1}{1+e^z}$$$g(z)$被称为$Sigmoid~Function$或$Logistic~Function$。其函数图像如下：
![[Pasted image 20241227164434.png]]
该函数的意义：
例如：当$$x=\left[\begin{array}\\
x_{0} \\
x_{1}
\end{array}\\\right]=\left[\begin{array}\\
1 \\
x_{1}
\end{array}\\\right]$$时，$h_{\theta}(x)=\hat{y}$，那么说明该输入值$x$有$\hat{y}$的几率属于正类。即在$x$和$\theta$的条件下，$y=1$的概率：$$h_{\theta}(x)=P(y=1|x;\theta)$$
### 决策边界
一般来说，我们会将$h_{\theta}(x)\geq 0.5$预测为$y=1$，而将$h_{\theta}<0.5$预测为$y=0$。
对于$\theta^Tx$而言，即是当$\theta^Tx<0$时，$y=0$；$\theta^Tx\geq 0$时，$y=1$。
决策边界的定义：
	作出$y=1$和$y=0$的预测的分界点，即为$h_{\theta}(x)=0.5$或$\theta^Tx=0$的线(分界线)。

### 代价函数
对于线性回归，我们可以列出其代价函数：$$J(\theta_{0}, \theta_{1})=\frac{1}{m}\sum_{i=1}^m\frac{1}{2}(h_{\theta}(x)^{(i)} - y^{(i)})^2$$将其写成如下形式：$$J(\theta_{0}, \theta_{1})=\frac{1}{m}\sum_{i=1}^m Cost(h_{\theta}^{(i)},y^{(i)})~~,~~Cost(h_{\theta}(x),y)=\frac{1}{2}(h_{\theta}(x) - y)^2$$对于逻辑回归，上述$Cost$函数就会成为一个非凸函数，不能保证它能收敛到全局最小值，因此修改$Cost$函数：$$J(\theta_{0}, \theta_{1})=\frac{1}{m}\sum_{i=1}^m Cost(h_{\theta}^{(i)},y^{(i)})$$$$Cost(h_{\theta}(x),y)=\left\{\begin{array} \\
-\log(h_{\theta}(x))~~~~~if~~y=1 \\
-\log(1-h_{\theta}(x))~~~~~if~~y=0
\end{array} \right.$$上半部分的代价函数图像：![[Pasted image 20241227171143.png]]
下半部分的代价函数图像：![[Pasted image 20241227171326.png]]
这个代价函数也可以写成如下形式($y=1或y=0$)，它和上面的那条是等价的：$$Cost(h_{\theta}(x),y)=-y\log(h_{\theta}(x))-(1-y)\log(1-h_{\theta}(x))$$
对于梯度下降算法，同样的，我们也是不断重复下列过程，直到收敛：$$\frac{\partial}{\partial \theta_{j}}J(\theta) =\frac{1}{m}\sum_{i=1}^m(h_{\theta}(x^{(i)})-y^{(i)})x^{(i)}$$
特征缩放算法也同样适用于逻辑回归。

### 多分类问题
多分类问题是介于回归问题和二分类问题之间的一类问题，它的作用在于将给定的输入值分为给定的几个类。
一般来说，可以把一个多分类问题拆分为几个独立的二分类问题来解决。


### 多分类问题解决过程
给每个二分类问题分别给定一个$h_{\theta}^{(i)}(x)$，预测$y=i$的概率，对于一个新的给定的输入$x$，选择$\hat{y}=h_{\theta}^{(i)}(x)$中$\hat{y}$最大的那个类作为最终的分类结果。


