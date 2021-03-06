# 贝叶斯滤波和卡尔曼滤波

## 1-2、先验概率和后验概率

### 1、贝叶斯滤波所处理的随机变量实际是随机过程中的变量

> **随机过程**（英语：Stochastic process）是[随机变量](https://zh.wikipedia.org/wiki/隨機變數)的集合。若一[随机系统](https://zh.wikipedia.org/w/index.php?title=随机系统&action=edit&redlink=1)的[样本点](https://zh.wikipedia.org/wiki/样本点)是随机函数，则称此函数为样本函数，这一随机系统全部样本函数的集合是一个**随机过程**。实际应用中，样本函数的一般定义在时间域或者空间域。随机过程的实例如[股票](https://zh.wikipedia.org/wiki/股票)和[汇率](https://zh.wikipedia.org/wiki/汇率)的波动、语音信号、视频信号、体温的变化，随机运动如[布朗运动](https://zh.wikipedia.org/wiki/布朗运动)、[随机徘徊](https://zh.wikipedia.org/wiki/随机徘徊)等等。

随机过程的简单表示：
$$
x_1,x_2,x_3,\cdot \cdot \cdot \qquad 其中x_1,x_2,\cdot\cdot\cdot \text 彼此之间不独立
$$
相对的，**确定过程**例子如下：
$$
e.g. \quad v=gt \qquad v_1=g,v_2=2g,\cdot\cdot\cdot ,\quad t=1,2,\cdot\cdot\cdot
$$
概率论与数理统计很多理论都是以**独立性**为前提，而现在所处理的**随机变量**没有**独立性**这种良好的性质。

### 2、变量之间缺乏独立性导致无法做随机试验、无法选定随机过程的初值

研究**随机过程**时，如果我们能找到以下两个变量之间的函数关系
$$
x_k=f(x_{k-1})
$$
那么我们就能找到它们之间概率的关系：
$$
P(x_k)=f(P(x_{k-1}))  \qquad:\text不独立性
$$
上述只是给了变量之间的关系，但是没有给出具体的**值**，因此还必须**选取一个初值**：
$$
P(x_0)=?
$$

------

**随机试验**：符合以下三个特点的试验叫做随机试验

> 1. 可以在相同的条件下重复的进行。
> 2. 每次试验的可能结果不止一个，并且能事先明确试验的所有可能结果。
> 3. 进行一次试验之前不能确定哪一个结果会出现。

其中，第一个特点是由变量之间的相互**独立性**来保证的。只有保证随机试验之间相互独立，才能使用**大数定律**做很多次试验，然后用**频率**来代替**概率**，才能给**概率**赋值。

**大数定律**定义：

>设μ是n次独立试验中事件A发生的次数，且事件A在每次试验中发生的[概率](https://baike.baidu.com/item/概率)为p，则对任意正数ε，满足：
>$$
>\lim _{n \rightarrow \infty} P\left(\left|\frac{\mu}{n}-p\right|<\varepsilon\right)=1
>$$

对于大量**随机过程**来说，由于变量之间没有相互**独立性**，所以无法利用重复的随机试验给概率赋值。换句话说，变量之间缺乏**独立性**导致无法选取随机过程的**初值**，也就无法研究该随机过程。

例如，股市是一种随机过程，因为今天的股市和昨天的股市相比不满足**随机试验**第一个特点：相同的条件、重复进行。由于它们之间不满足相互**独立性**，如果想要今天的股市重复昨天股市的条件，就需要坐时光机回到昨天，这是不可能完成的事情。

所以，能做**随机试验**的变量一般都是和时间没什么关系的。

> 有的随机过程初值可以给出，可以做随机试验，比如**随机游走**，可以给一个初值（下标为时间t）：
> $$
> x_k=x_{k-1}+D
> $$
> 表示：t=k时刻的位移等于k-1时刻的位移加从 k-1时刻到k时刻之间移动的位移，其中D为随机变量：
> $$
> D \backsim\left\{\begin{array}{l}
> P\left(\text 向前走1米\right)=\frac{1}{2} \\
> P\left(\text 向后走1米\right)=\frac{1}{2}
> \end{array} \quad \text {人为规定起点，初值：} P\left(x_{0}=0\right)=1\right.
> $$
> 即认为刚开始在空间位置0处开始随机游走，这是一个必然事件，概率为1。

虽然类似**随机游走**的随机过程可以人为规定**初值**（该初值和时间无关），但是大量的随机过程的初值是与时间相关的，而时间是不可倒流的。

### 3、选定初值的方法

> 主观概率，是指建立在过去的经验与判断的基础上，根据对未来事态发展的预测和历史统计资料的研究确定的概率。 反映的只是一种主观可能性。 尽管有一定的科学性，但和能客观地反映事物发展规律的自然概率不同。 自然概率的科学性已为过去若干统计结果所证实，而主观概率只能反映未来事件发生的近似可能性。

由于无法使用随机试验以频率赋值概率确定随机过程的初值。这时候，就只能使用**主观概率**。即猜一个概率出来，给一个大概的初值。 

这样主观估计的初值肯定是不靠谱的，因此需要引入外部观测（证据、信息）来纠正（即，乘**似然概率**），使该初值变为相对客观的概率，这个概率就叫做——**后验概率**，即实验观测之后的概率。

而之前的主观概率就叫做——**先验概率**，即实验观测之前的概率。（PS. 实际上，这里有略微区别。主观概率纯粹是猜的，而先验概率是通过经验、查资料等获取的并没有做实验验证的概率）
$$
\text 先验概率 \overset{\text 外部观测}{\rightarrow} \text后验概率
$$

> 先验概率包含各种噪声。后验概率是通过观测来修正原始信号，所以称之为 “滤波”。

## 3、似然概率

### 1、规定

$$
X,Y \quad \text 代表随机变量，\qquad x,y \quad \text 随机变量的取值，代表随机试验一个可能的结果
$$

### 2、引例

温度：今天多少度？（T 代表温度，Tm代表温度计测量温度）

首先，给出完整的**先验概率分布**，猜一下今天多少度（假设只有这两种温度的情况）：
$$
\left\{\begin{array}{l}
P(T=10)=0.8 \\
P(T=11)=0.2
\end{array}\right.
$$
其次，温度计测量得：
$$
T_{m}=10.3 \qquad \text 其中，m为\ measure
$$
最后，由贝叶斯公式，后验概率分布：
$$
\begin{array}{l}
P\left(T=10 \mid T_{m}=10.3\right)=\frac{P\left(T_{m}=10.3 \mid T=10\right) P(T=10)}{P\left(T_{m}=10.3\right)} \\
P\left(T=11 \mid T_{m}=10.3\right)=\frac{P\left(T_{m}=10.3 \mid T=11\right) P(T=11)}{P\left(T_{m}=10.3\right)}
\end{array}
$$

### 3、分析

其中，等式右边的左侧分子：
$$
P\left(T_{m}=10.3 \mid T=10\right)
$$
称为**似然概率**，代表了**观测的准确度**，或传感器的精度。（即，在真实温度为10时，假设温度计测量误差为±0.5，那么测量值为10.3的概率就会很大；假设温度计测量误差为±0.1，那么测量值为10.3的概率就会很小。也就是说在真实温度为10时，假设温度计测量误差为±0.1，使得似然概率最大即最有可能观测到的的温度计读数Tm会落在9.9~10.1之间。）

反过来说（见5、补充），如果我们温度计观测值为10.3，且温度计测量误差为±0.3，那么有：
$$
P\left(T_{m}=10.3 \mid T=10\right) >> P\left(T_{m}=10.3 \mid T=11\right)
$$
即，在上述条件的观测值下，最有可能的真实温度是 T=10。

另一个，等式右边的分母：
$$
P\left(T_{m}=10.3\right)\\
P\left(T_{m}=10.3\right)=P\left(T_{m}=10.3 \mid T=10\right) P(T=10)+P\left(T_{m}=10.3 \mid T=11\right) P(T=11)
$$
为温度计测量值为10.3时的概率。这个概率与T的取值无关，与T的分布律有关。T=10和T=11代表随机试验的一个结果，试验结果不会影响到分布律。而随着先验概率分布的设置，分布律是确定的。所以该分母为一个常数η。

### 4、后验概率=η\*似然概率*先验概率

$$
\text 后验 = \eta * 似然 * 先验 \\
\because \sum 后验 = \eta * \sum 似然 * 先验 \quad,\quad \sum 后验=1,\\
\therefore \eta = \frac{1}{\sum 似然 * 先验}
$$

### 5、补充

**似然**：likelihood，可能性，相似，像，源自于最大似然估计。表示：**哪个原因最有可能（最像）导致了这个结果**。

**似然概率**：状态取得当前观测值的概率

**最大似然估计**思想：在状态X的取值范围内找一个x，使得似然函数概率最大。概率越大的事件出现的频率越高，那么现在既然出现了当前观测值，那么就有理由认为出现当前观测值的概率大。那么使似然概率最大的那个状态，就是当前最有可能的状态。

- 举例：A班99男1女，B班99女1男。现在随机抽一个班（状态），再从此班抽一个人进行观测，结果是女（观测值）。那么此女（观测值）最可能（最像）是从B班（状态）抽出来的。

-----

$$
P(\underset{因}{状态} \mid \underset{果}{观测}) = \eta * P(\underset{果}{观测} \mid \underset{因}{状态}) * P(状态) \\
$$

**主观概率**：直接猜测的概率

**先验概率**：根据以往经验和分析得到的概率，**但并没有经实验验证**，在很多随机过程中，由于缺乏独立性，也无法进行随机试验验证

**后验概率**：由果推因，当前观测值对应的状态概率

**似然概率**：由因推果，状态取得当前观测值的概率

后验概率分布一般为：
$$
P(状态1 \mid 观测) \\
P(状态2 \mid 观测) \\
$$
似然概率分布一般为：
$$
P(观测 \mid 状态1) \\
P(观测 \mid 状态2) \\
$$

----

独立未必没有函数关系。假设Y=f(X)，Y与X可能独立，也有可能不独立。

比如，

1. 必然事件：Y=X+1，P(X=1)=1，P(Y=2)=1，P(X=1,Y=2)=1是独立的
2. 随机事件：设有一个正态的概率分布，期望和方差未知。在此分布中，抽取n个独立的样本，X1，……，Xn，则X1，……，Xn独立同分布，则随机变量的样本均值和样本方差是相互独立的。假、

## 4、贝叶斯公式

$$
\begin{array}{l}离散：
P(X=x \mid Y=y)=\frac{P(Y=y \mid X=x) P(X=x)}{P(Y=y)}=\eta*P(Y=y \mid X=x) P(X=x)=\eta*似然*先验 \\连续：
P(X<x \mid Y=y)=\int_{-\infty}^{x} \frac{f_{Y|X}\left(y|x)f_{X}(x)\right.}{f_{Y}(y)} dx=\int_{-\infty}^{x} f_{X \mid Y}(x \mid y) dx=\eta*f_{Y|X}(y|x)*f_{x}(x)
\end{array}
$$

## 5、总结



