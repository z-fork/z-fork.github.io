---
title: Gaussian-distribution_T-distribution
layout: post
tags:
  - 正态分布
  - t分布
---

**若 \\( X_1 \\), \\( X_2 \\) 独立, \\( X_1 \sim \chi_n^2 \\) 独立, \\( X_2 \sim N(0, 1) \\), 而 \\( Y = \frac{X_2}{\sqrt{\frac{X_1}{n}}} \\). 求 \\( Y \\) 的密度函数.**

记 \\( Z = \sqrt{\frac{X_1}{n}} \\). 先求出 \\( Z \\) 的密度函数 \\( g(z) \\)

$$
P(Z \leq z) = P(\sqrt{\frac{X_1}{n}} \leq z) = P(X_1 \leq nz^2) = \int_{0}^{nz^2}k_n(x)dx
$$

两边对 \\( Z \\) 求导, 得 \\( Z \\) 的密度函数为

$$
g(z) = 2nzk_n(nz^2)
$$

以 \\( f_1(x_1) = 2nx_1k_n(n{x_1}^2) \\) 和 \\( f_2(x_2) = \sqrt{2 \pi}^{-1}e^{-\frac{x_2^2}{2}} \\), 得 \\( Y \\) 的密度函数, 记为 \\( t_n(y) \\)

----
附 1:

**随机变量商的密度函数**

设 \\( (X_1, X_2) \\) 有密度函数 \\( f(x_1, x_2) \\), \\( Y = \frac{X_2}{X_1} \\). 求 \\( Y \\) 的密度函数. 限制 \\( X_1 > 0 \\)

考虑事件

$$
\{Y \leq y\} = \{\frac{X_2}{X_1} \leq y\}
$$

按密度函数的定义有

$$
P(Y \leq y) = P(\frac{X_2}{X_1} \leq y) = \iint\limits_Bf(x_1, x_2)dx_1dx_2
$$

积分区域 \\( B \\) 为 \\( X_1 - X_2 \\) 平面中 \\( X_2 = X_1y \\) 直线下方部分 且 \\( X_1 > 0 \\).

先固定 \\( x_1 \\) 对 \\( x_2 \\) 积分, 积分范围为 \\( -\infty \\) 到 \\( x_1y \\), 再对 \\( x_1 \\) 从 \\( 0 \\) 到 \\( \infty \\) 积分.

$$
P(Y \leq y) = \int_{0}^{\infty} [\int_{-\infty}^{x_1y}f(x_1, x_2)dx_2]dx_1
$$

对 \\( y \\) 求导数, 得 \\( Y \\) 的密度函数为

$$
l(y) = \int_{0}^{\infty}x_1f(x_1, x_1y)dx_1
$$

若  \\( (X_1, X_2) \\) 独立, 则 \\( f(x_1, x_2) = f(x_1) \cdot f(x_2) \\)

$$
l(y) = \int_{0}^{\infty}x_1f_1(x_1)f_2(x_1y)dx_1
$$

----

$$
\begin{matrix}
t_n(y) & = & \sqrt{2 \pi}^{-1}(2^{\frac{n}{2}} \Gamma(\frac{n}{2}))^{-1} \int_{0}^{\infty}2nx_1^2e^{-\frac{nx_1^2}{2}}(nx_1^2)^{\frac{n-2}{2}} \cdot e^{-\frac{(x_1y)^2}{2}}dx_1 \\
       & = & \sqrt{2 \pi}^{-1}(2^{\frac{n}{2}} \Gamma(\frac{n}{2}))^{-1} \cdot \int_{0}^{\infty}x_1^n exp[-\frac{1}{2}(nx_1^2 + x_1^2y^2)]dx_1 \\
       & = & \frac{ \Gamma(\frac{n+1}{2})}{\sqrt{n \pi} \Gamma(\frac{n}{2})}(1 + \frac{y^2}{n})^{-\frac{n+1}{2}}
\end{matrix}
$$

这个密度函数称为 "自由度 n 的 t分布" 的密度函数, 记为 \\( Y \sim t_n \\).

----
----
