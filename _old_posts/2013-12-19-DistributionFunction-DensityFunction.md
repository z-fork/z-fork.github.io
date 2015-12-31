---
title: Distribution-function_Density-function
layout: post
tags:
  - 分布函数
  - 密度函数
---

**定义 1.3** 设连续性随机变量 \\( X \\) 有概率分布函数 \\( F(x) \\), 则 \\( F(x) \\)的导数 \\( f(x) = F'(x) \\), 称为 \\( X \\) 的概率密度函数.

1. \\( f(x) \geq 0 \\)
2. \\( \int_{-\infty}^{\infty}f(x)dx = 1 \\)
3. 对任何常数 \\( a < b \\) 有
$$
P(a \leq X \leq b) = F(b) - F(a) = \int_{a}^{b}f(x)dx
$$

