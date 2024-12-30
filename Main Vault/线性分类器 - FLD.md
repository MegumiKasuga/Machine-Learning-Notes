FLD模型，即**Fisher's Linear Disctiminant —— Fisher线性判别分析**。
FLD模型是线性判别分析(**Linear Discriminant Analysis, LDA模型**)的一种。

### FLD模型的目的
给定一个投影矢量$\omega$，将$x$投影到$\omega$矢量上，使得不同类别的$x$投影后的值$y=\omega^Tx$尽可能相互远离。
![[Pasted image 20241229193739.png]]
当原始数据线性可分时，就能够求出这个最优的投影矢量。
**两个准则**：
- **投影后的两类样本均值之间的距离尽可能大。**
- **投影后的两类样本各自的方差尽可能小。**

### 二分类问题
假设有一个训练集$(x_{i}, c_{i}),~i=1,\dots,N$其中，$c_{i}\in 1,2$。
$X_{1}=x_{i}|1\leq i \leq N~~~,~~~c_{i}=1,X_{2}=x_{i}|1\leq i\leq N,c_{i}=2$
$N_{1}$为$X_{1}$中的元素个数；$N_{2}$为$X_{2}$中的元素个数。

投影矢量$\omega$满足$|\omega|=1$，投影后的样本矢量为：$$y_{i}=\omega^T x~~,~~i=1,\dots,n$$$$Y_{1}=y_{i}|i\leq i\leq N, c_{i}=1~~~,~~~Y_{2}=y_{i}|1\leq i \leq N,c_{i}=2$$在二分类中，$y=\omega^T x$，其中$y$为一个标量，而$\omega$和$x$均为矢量。
投影之后的样本均值：$$\tilde{m}_{k}=\frac{1}{N_{k}}\sum_{y \in Y_{k}}y=\frac{1}{N_{k}}\sum_{x \in X_{K}}\omega^T x$$$$\tilde{m}_{k}=\omega^Tm_{k},k \in \{1,2\}$$原样本空间的样本方差：$$s_{k}^2=\frac{1}{N_{k}}\sum_{x \in X_{k}}(x-m_{k})^2,k \in \{1,2\}$$
注意：$||\tilde{m}_{1}-\tilde{m}_{2}||=||\omega^T m_{1}-\omega^T m_{2}||=||\omega^T||||m_{1}-m_{2}||=||m_{1}-m_{2}||$
那么，Fisher准则函数就是求下列式子：$$max\frac{|\tilde{m}_{1}-\tilde{m}_{2}|}{\tilde{s}_{1}^2+\tilde{s}_{2}^2}$$
定义$X_{1}$和$X_{2}$的类内散度矩阵$$S_{1}=\sum_{x \in X_{1}}(x-m_{1})(x-m_{1})^T~~~,~~~S_{2}=\sum_{x \in X_{2}}(x-m_{2})(x-m_{2})^T$$
那么，准则函数可以重新写成(在数学和物理上，这也被称为瑞利商)：$$J(\omega)=\frac{\omega^TS_{B}\omega}{\omega^TS_{W}\omega}$$其中$S_{B}=(m_{1}-m_{2})(m_{1}-m_{2})^T$，称为总类内散度矩阵；$S_{W}=S_{1}+S_{2}$，称为总类间散度矩阵。

### 优化
令$\frac{\partial}{\partial \omega}J(\omega)=0$，那么可以得出：$$(\omega^T S_{B} \omega)S_{W}\omega=(\omega^T S_{W} \omega)S_{B}\omega$$又$S_{B}\omega=(m_{2}-m_{1})(m_{2}-m_{1})^T\omega=\alpha(m_{2}-m_{1})$，其方向总和$m_{2}-m_{1}$的方向一致(另外那一块是标量)，那么我们可以得出：$$\omega \propto S_{W}^{-1}(m_{1}-m_{2})$$