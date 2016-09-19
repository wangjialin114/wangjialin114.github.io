---
layout:     post
title:      "Boosting系-AdaBoost"
subtitle:   "三个臭皮匠，顶个诸葛亮"
date:       2016-09-18
author:     "WangJialin"
tags:
- Machine Learning
- Boosting
---

首先我们来看看集成学习拥有魅力的原因。

**在PCA（概率近似正确）的框架下，如果一个概率（一个类）可以在多项式的时间内学习到，并且正确率很高，那么就称这个概念是强可学习的;一个概念，如果存在一个多项式的学习算法能够学习它，学习的正确率仅比随机猜测略好，那么就称这个概念是弱可学习的.**

Schapire后来证明强可学习与弱可学习是等价的，也就是说，在PAC学习的框架下，一个概念是强可学习的充分必要条件是这个概念是弱可学习的。

注意,上面的弱可学习时的正确率一定要比随机猜测好.这个是很重要,不然的话,集成学习是不能work的.

集成(ensemble)学习现在用的越来越广泛，各大比赛的利器啊。有必要大叔特书的。在这里，我将分几个系列进行讲解：

- Boosting系列
- Bagging与Random Forest
- 结合策略

每个系列都有很多东西可说，Boosting系列和Bagging和Random Rorest集成的是同类的学习器；而结合策略则是将不同的学习器结合。其中Boosting中，学习器迭代的时候关注的前面学习器预测错误的样本，而Bagging和Random Forest则是利用学习的时候样本取样的随机性和决策树选择划分属性的随机性。

下面我首先来介绍Boosting系列中最早的AdaBoost。

### 基本思想

****

**AdaBoost在迭代过程中，新(T+1)的学习器总是更关注前面学习器(T)分错的样本，将前面分错的样本的权重变大，分队的样本变小。类似于一种残差逼近的思想。**

****

### 算法
输入：$$D:\{ (x_1,y_1),...,(x_m,y_m) \}$$,基学习算法,训练次数T

```
#初始化数据分布
每个点的初始权重1/N
for t=1,2,...,T
    利用基学习算法得到基学习器
    计算基学习器的错误率
    计算基学习器的权重
    更新数据权重,注意归一化
end
```

输出:$$H_t(x) = \sum_{i=1}^{t}\alpha_th_t(x)$$


因此问题的关键在于求得每轮学习时的权重$$\alpha_t$$，进行数据分布的更新。

先摆结论：

- 计算$$\alpha_t$$

$$\alpha_t=\frac{1}{2}ln(\frac{1-\epsilon_t}{\epsilon_t})$$

- 对于每个数据点$$(x_i,y_i)$$，其权重$$w_{i}$$更新的方式为：

 $$h_t(x)=f(x)$$,预测正确，降低权重，$$w_{i}=w_{i}*e^{-\alpha_t}$$

 $$h_t(x)\neq(x)$$,预测错误，增加权重，$$w_{i}=w_{i}*e^{\alpha_t}$$

- 归一化$$w_{i}$$

$$w_{i}=\frac{w_{i}}{\sum_{j=1}^{m}w_{j}}$$

下面是数学推导，没兴趣的同学可以跳过。

### 数学推导

我们以二分类为例，样本标签为$$\{ +1, -1 \}$$。

$$H_t(x) = \sum_{i=1}^{t}\alpha_th_t(x)$$

令损失函数为指数函数，$$f(x)$$是真实函数，则此学习器造成的损失为：

$$L(H_t|D)=E_{x \sim D}[e^{-H_t(x)f(x)}]$$

$$
\begin{align}
\frac{\partial L(H_t|D)}{\partial H_t(x)}&=E_{x \sim D}[-f(x)*e^{-H_t(x)f(x)}] \\
							        &=P(f(x)=-1|x)e^{H_t(x)}-P(f(x)=1|x)e^{-H_t(x)} \\
\end{align}
$$

令导数为0,则求得：

$$H_t(x)=\frac{1}{2}ln\frac{P(f(x)=-1|x)}{P(f(x)=+1|x)}$$

由上式可知，AdaBoost达到了贝叶斯最优错误率。

接下来的问题，就是如何求$$\alpha_t$$了。OK，我们继续上面的套路：

$$L(\alpha_t h_t|D_t)=E_{x \sim D_t}[e^{-\alpha_t h_tf(x)}]$$

$$
\begin{align}
\frac{\partial L(\alpha_t h_t|D_t)}{\partial \alpha_t}&=E_{x \sim D}[-f(x)h_t(x)*e^{-\alpha_t h_tf(x)}] \\
							        &=-P_{x \sim D_t}(f(x)=h_t(x))e^{-\alpha_t}+P_{x \sim D_t}(f(x)\neq h_t(x))e^{\alpha_t} \\
&=-(1-\epsilon_t)e^{-\alpha_t}+\epsilon_te^{\alpha_t}
\end{align}
$$

令导数为0,则求得：
$$\alpha_t=\frac{1}{2}ln(\frac{1-\epsilon_t}{\epsilon_t})$$

- 对于每个数据点$$(x_i,y_i)$$，其权重$$w_{i}$$更新的方式为：

 $$h_t(x)=f(x)$$,预测正确，降低权重，$$w_{i}=w_{i}*e^{-\alpha_t}$$

 $$h_t(x)\neq(x)$$,预测错误，增加权重，$$w_{i}=w_{i}*e^{\alpha_t}$$

- 归一化$$w_{i}$$

$$w_{i}=\frac{w_{i}}{\sum_{j=1}^{m}w_{j}}$$

### 参考文献

[1] 机器学习, 周志华
[2] 统计学习方法, 李航