# 机器学习系列笔记 - 贝叶斯分类器

这是分类问题的第一个常见的方法，作为整个笔记的第一个内容。这是一个数理统计有关的方法。

## 贝叶斯定理

$$
P(A|B) = \frac{P(B|A)P(A)}{P(B)}
$$

其中：

- $P(A|B)$是在给定B的情况下A的概率（后验概率，Posterior Probability）
- $P(B|A)$是在给定A的情况下B的概率（条件概率，Conditional Probability；或者称为似然度，Likelihood）
- $P(A)$是A的概率（先验概率，Prior Probability）
- $P(B)$是B的概率。这个数字后续再说。

对于上述公式，将其更一般化，可以得到：

$$
P(c_i|x) = \frac{P(x|c_i)P(c_i)}{\sum_{j=1}^{n}P(x|c_j)P(c_j)}
$$

其中，类条件概率（Class conditional probability）$P(x|c_i)$是指在类别$c_i$下样本$x$的概率，先验概率$P(c_i)$是指类别$c_i$的概率，后验概率$P(c_i|x)$是指在给定样本$x$的情况下类别$c_i$的概率。

可以得到，对于一个二分类问题，比如存在$A$和$A'$两个类别，那么对于一个样本$B$，可以得到：

$$
P(A|B) = \frac{P(B|A)P(A)}{P(B)} = \frac{P(B|A)P(A)}{P(B|A)P(A) + P(B|A')P(A')}
$$

到这里，公式的推导还是很显然的。至于具体各种概率如何计算，后续再说。

## 贝叶斯分类器

