---
layout: post
title: Cross Entropy Loss
categories: AI
description: 交叉熵
keywords: 交叉熵, 损失函数
---
神经网络中经常使用交叉熵作为损失函数，为什么选择交叉熵？透过表象看本质！

## 交叉熵
作用：**交叉熵就是用来判定实际的输出与期望的输出的接近程度！**

### 数学原理
在神经网络分类问题中，二分类问题中常用sigmoid函数将上一层输出映射成正类的概率，多分类问题中常用softmax函数映射分类概率。

sigmoid函数，以伯努利分布建模，样本为1的概率：s=0时，输出0.5；(0, 1)
$$
f(y = 1|x; \theta) =  \frac{1}{1+e^{-\theta x}} = g(s) = \frac{1}{1+e^{- s}}
$$
softmax函数，以多项式分布建模，样本类别为2时，退化为sigmoid函数：
$$
p(y^{(i)}=j|x^{(i)}; \theta) = \frac {e^{\theta x^{i}}} {\sum_k {e^{\theta x^{k}} }} = p(s^{i}) = \frac {e^{s^{i}}} {\sum_k {e^{s^{k}} }}
$$

#### 最大似然(Maximum-Likelihood)准则
指导我们从不同模型中得到特定函数作为好的估计，而不是猜测某些函数可能是好的估计，然后分析其偏差和反差。其思想是利用已知的样本，反推出最大概率出现这种结果的参数值，即通过观察已知的样本，评定模型好坏。

考虑有一组含m个样本的数据集X = {x1, x2, x3, , , xm}，独立地由未知的真实数据生成分布$P_{data}(x)$生成。

令$P_{model}(x; \theta)$是一族$\theta$确定在相同空间上的概率分布。换言之，$P_{model}(x; \theta)$将任意输入x映射到实数来估计真实概率$P_{data}(x)$。

令$\overline{P}_{data} {(x)}$为训练数据经验分布，当训练样本足够多时，经验分布可以近似接近数据真实分布，训练模型的做法是最小化$P_{model}(x; \theta)$与$\overline{P}_{data} {(x)}$的差异。

样本独立分布，对$\theta$的最大似然估计定义为(联合概率分布)：
$$
\theta_{ML} = {\arg\max}_{\theta} \ \ p_{model}(X; \theta) = {\arg \max}_{\theta} \prod_{i=1}^{m} p_{model}(x^{(i)};\theta)
$$
上式可以理解为参数为$\theta$的模型分布使样本集出现的概率最大。
为了方便计算，转换为似然对数，将乘积转化为求和形式：
$$
\theta_{ML} = {\arg\max}_{\theta} \ \ p_{model}(X; \theta) = {\arg \max}_{\theta} \sum_{i=1}^{m} \log p_{model}(x^{(i)};\theta)
$$

使用KL散度来量化模型分布与经验分布之间的差异，即相对熵。

$$
D_{KL}(\overline{p}_{data} || p_{model}) = \sum_{i=1}^N \overline{p}_{data}(x_i) (\log{\overline{p}_{data}(x_i)} - \log p_{model}(x_i)) \\= E_{x-\overline{p}_{data}} {(\log{\overline{p}_{data}(x_i)} - \log p_{model}(x_i) )}
$$

左边一项涉及数据生成过程，是常数，与模型无关。即意味着最小化KL散度时，只需要最小化
$$
E_{x-\overline{p}_{data}} {(- \log p_{model}(x_i))}
$$

上式即定义为交叉熵。因此，负对数似然即定义在训练集上的经验分布和定义在模型上的概率分布之间的交叉熵。如均方误差是经验分布和高斯模型之间的交叉熵。sigmoid的负对数似然是经验分布和伯努利模型之间的交叉熵。softmax是和多项式分布之间的交叉熵。

回到sigmoid分布，根据最小化负对数似然准则，可以得到二分类下交叉熵损失：
$$
L =  - (\overline{p}_{data}(x=1) \log p_{model}(x=1)\\ + (1-\overline{p}_{data}(x=1) ) (1-\log p_{model}(x=1) ) )
$$

#### 条件对数似然
拓展到估计条件概率$P(Y|X;\theta)$，从而给定x预测y。这就构成了常见的有监督学习的基础。最大似然估计是：
$$
\theta_{ML} = {\arg\max}_{\theta} \ \ p_{model}(Y|X; \theta)
$$

#### 最大似然的性质
当样本数目趋向无穷大时，就收敛而言时最好的渐近估计。
在合适的条件下，最大似然估计具有一致性，这些条件是：
* 真实分布必须在模型族中，否则，没有估计可以还原真实分布；
* 真实分布必须刚好对应一个$\theta$值，否则最大似然估计无法决定数据生成过程使用哪个$\theta$值。

## 后话
书到用时方恨少，每天积累一点点，每天进步一点点！

## 参考
