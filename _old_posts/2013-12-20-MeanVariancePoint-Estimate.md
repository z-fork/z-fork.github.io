---
title: Mean Variance Point-Estimate
layout: post
tags:
  - 均值
  - 方差
  - 估计量
  - 点估计
  - 无偏估计
  - 自由度

---

**若 \\( X_1, \cdots, X_n \\) 是从某总体中抽出的样本, 则样本均值 \\( \bar{X} \\) 是总体分布均值 \\( \theta \\) 的无偏估计.**

根据定义, **每个样本 \\( X_i \\) 的分布, 与总体分布一样**, 因此其均值 \\( E(X_i) \\) 就是 \\( \theta \\)

----
附 1:

> 期望的运算

----

$$
E(\bar{X}) = E(\frac{\sum_{i=1}^{n}X_i}{n}) = \frac{\sum_{i=1}^{n}E(X_i)}{n} = \frac{n \theta}{n} = \theta
$$

\\( \bar{X} \\) 是总体分布均值 \\( \theta \\) 的无偏估计.

----
----

**若 样本方差 \\( S^2 = \frac{\sum_{i=1}^{n}(X_i - \bar{X})^2}{n-1} \\), 是总体分布方差 \\( \sigma^2 \\) 的无偏估计.**

以 \\( a \\) 记总体分布均值: \\( E(X_i) = a \\). 也有 \\( E(\bar{X}) = a \\), 把 \\( X_i - \bar{X} \\) 写为 \\( (X_i - a) - (\bar{X} - a) \\)

$$
\begin{matrix}
\sum_{i=1}^{n}(X_i - \bar{X})^2 & = & \sum_{i=1}^{n}[(X_i - a) - (\bar{X} - a)]^2 \\
                                & = & \sum_{i=1}^{n}(X_i - a)^2 - 2(\bar{X} - a)\sum_{i=1}^{n}(X_i - a) + n(X_i - a)^2
\end{matrix}
$$

注意到 \\( \sum_{i=1}^{n}(X_i - a) = \sum\_{i=1}^{n}X_i - na = n\bar{X} - na = n(\bar{X} - a) \\)

$$
\sum_{i=1}^{n}(X_i - \bar{X})^2 = \sum_{i=1}^{n}(X_i - a)^2 - n(X_i - a)^2
$$

----
附 2:

> 方差的运算

----

因 \\( a = E(X_i) = E(\bar{X}) \\)

$$
E(X_i - a)^2 = Var(X_i) = \sigma^2, i = 1, \cdots, n
$$

$$
E(X_i - a)^2 = Var(\bar{X}) = Var(\frac{\sum_{i=1}^{n}X_i}{n}) = \frac{\sum_{i=1}^{n}Var(X_i)}{n^2} = \frac{n \sigma^2}{n^2} = \frac{\sigma^2}{n}
$$

于是得到

$$
E(S^2) = \frac{1}{n-1}E(\sum_{i=1}^{n}(X_i - \bar{X})^2) = \frac{1}{n-1}(n \sigma^2 - n \cdot \frac{\sigma^2}{n}) = \sigma^2
$$

\\( S^2 \\) 是 \\( \sigma^2 \\) 的无偏估计.

----
----

> 这就解释了为什么要在样本二阶中心矩 \\( m_2 = \frac{\sum_{i=1}^{n}(X_i - \bar{X})^2}{n} \\) 的基础上, 把分母 \\( n \\) 修正为 \\( n-1 \\) 以得到 \\( S^2 \\). 

> 在这里可以对"自由度"这个概念赋予一种解释: 一共有 \\( n \\) 个样本, 有 \\( n \\) 个自由度. 用 \\( S^2 \\) 估计方差 \\( \sigma^2 \\), 自由度本应为 \\( n \\). 但总体均值 \\( a \\) 也未知, 用 \\( \bar{X} \\) 去估计之, 用掉了一个自由度, 故只剩下 \\( n-1 \\) 个自由度.

> 如果总体均值 \\( a \\) 已知, 则不用 \\( S^2 \\) 而用 \\( \frac{\sum_{i=1}^{n}(X_i - a)^2}{n} \\) 去估计总体方差 \\( \sigma^2 \\) (在 \\( a \\) 未知时不能用). 这是 \\( \sigma^2 \\) 的无偏估计, 分母为 \\( n \\) 不用改为 \\( n-1 \\). 因为此处 \\( n \\) 个自由度全保留下了( \\( a \\) 已知, 不用估计, 没有用去自由度).

----
----