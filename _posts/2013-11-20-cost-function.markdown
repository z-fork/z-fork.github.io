---
title: Cost function in Regression
layout: post
tags:
- cost function
- regression
---

#### What?

借助 cost function 求出参数  使更合理的满足 traning set 中的所有 training examples.

在 Linear Regression ( 线性回归 ) 中 hypothesis ( 假设函数 ) 如下:

\\[ h_\theta(x) = \theta_0 + \theta_1x_1 + \theta_2x_2 + \cdots + \theta_nx_n \\]

设 \\( x_0 = 1 \\) 则 \\( h_\theta(x) = \theta^TX \\).

Cost function 表示 input values or feature 通过 hypothesis 得出 \\( \theta^{T}X \\) 与实际 \\( Y \\) 之间关系.

抛开 cost function, 想一想怎么求 \\( \theta^{T}X \\) 等于或无穷接近 \\( Y \\) 的 \\( \theta \\).

#### Methods

  * M0  对 \\( \theta \\) 穷举, 使 \\( \theta^TX \\) 接近 \\( Y \\). ( long long time )
  * M1  给 \\( \theta \\) 一个初始值, 不断更新 \\( \theta \\) 值, 直到 \\( \theta^TX \\) 接近 \\( Y \\).( 有限的迭代 )
  * M2  直接计算 \\( \theta \\) 值.

#### How

  * M1

1. 先考虑一个问题.

某工厂生产汽车零件, 零件规格 100mm 为合格品, 现有每批5个, 共A,B两批生产出的零件.

A: 100mm, 101mm, 100mm, 99mm, 100mm

B: 102mm, 100mm, 90mm, 110mm, 98mm

问: 哪一批零件更好一些.

> 期望相同时, 用方差判断波动.

我们以 \\( h_\theta(x) \\) 与 \\( Y \\) 的差的平方和作为 Cost function, 为了计算方便乘一个 \\( \frac{1}{2} \\).

$$ J(\theta) = \frac{1}{2}\sum_{i=1}^n(h_\theta(x^{(i)}) - y^{(i)})^2 $$

寻找一个 \\( \theta \\) 使 \\( J(\theta) \\) 取最小值, 使 \\( h_\theta X \rightarrow Y \\).

计算 \\( J(\theta) \\) 对 \\( \theta \\) 偏导求"斜率", 如在坡上, 沿着最陡峭的方向最快下坡

$$ \begin{matrix}
\frac{\partial}{\partial \theta_j} J(\theta) & = & 2 \cdot \frac{1}{2} \cdot (h_\theta(x-y)) \cdot \frac{\partial}{\partial \theta_j}(h_\theta(x) - y) \\
 & = & (h_\theta(x)-y) \cdot \frac{\partial}{\partial \theta_j}(\sum_{i=0}^n\theta_ix_i-y) \\
 & = & (h_\theta(x)-y) \cdot x_j
\end{matrix} $$

于是得出迭代公式:

$$ \theta_j := \theta_j + \alpha (y^{(i)}) - h_\theta(x^{(i)})) x_j^{(i)} $$

此公式中 \\( \alpha \\) 为步长, 下坡时一步迈多长

不断迭代求出收敛的 \\( \theta \\), 得出 hypothesis function

\\[ h_\theta(x) = \theta_0 + \theta_1x_1 + \theta_2x_2 + \cdots + \theta_nx_n \\]



----

证明另外会写. 关于为何这么取 cost function 以后会另解释.
