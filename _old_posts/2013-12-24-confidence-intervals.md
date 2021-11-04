---
title: Confidence Intervals
layout: post
tags:
  - 置信区间
  - 小样本法
  - 大样本法
  - 置信界

---

**给定一个很小的数 \\( \alpha > 0 \\). 如果对参数 \\( \theta \\) 的任何值, 概率都等于 \\( 1-\alpha \\), 则称区间估计 \\( [\hat{\theta}_1, \hat{\theta}_2] \\) 的置信系数为 \\( 1-\alpha \\).**

区间估计也常称为"置信区间". 对该区间能包含未知参数 \\( \theta \\) 可置信到何种程度.

**枢轴变量法**

\\( X_1, \cdots, X_n \\) 为抽自正态总体 \\( N(\mu, \sigma^2) \\) 的样本, \\( \sigma^2 \\) 已知, 要求 \\( \mu \\) 的区间估计.

先找一个 \\( \mu \\) 的良好的点估计. 选择样本均值 \\( \bar{X} \\). 由总体为正态易知

$$
\sqrt{n}(\bar{X} - \mu)/\sigma \sim N(0, 1)
$$

以 \\( \Phi \\) 记 \\( N(0, 1) \\) 的分布函数. 对 \\( 0 < \beta < 1 \\)(一般是\\( \beta \\)很小), 用方程

$$
\Phi(u_\beta) = 1 - \beta
$$

定义记号 \\( u\_\beta \\). \\( u\_\beta \\) 称为分布 \\( N(0, 1) \\) 的 "上 \\( \beta \\) 分位点".

其意义是: \\( N(0, 1) \\) 分布中大于 \\( u_\beta \\) 的那部分的概率, 就是 \\( \beta \\).

[![](/files/2013/zxqj.png)](http://mongoo.cn)

画出的是 \\( N(0, 1) \\) 的密度函数 \\( \varphi(x) = (\sqrt{2 \pi})^{-1}e^{-x^2/2} \\) 的图形, 涂黑部分标出的面积为 \\( \beta \\).

上 \\( \beta \\) 分位点的概念可推广到任何分布 \\( F: \\) 满足条件 \\( F(v\_\beta) = 1 - \beta \\) 的点 \\( v\_\beta \\), 就是分布函数 \\( F \\) 的上 \\( \beta \\) 分位点.

除了正态分布外, "统计三大分布"的上分位点很常用. \\( x_n^2(\beta) \\), \\( t_n(\beta) \\) 和 \\( F_{n,m}(\beta) \\) 这些都有表可查.

回到 \\( \mu \\) 的区间估计, 注意到 \\( \Phi(-t) = 1 - \Phi(t) \\), 有

$$
\begin{matrix}
 & P(-u_{\alpha/2} \leq \sqrt{n}(\bar{X} - \mu)/\sigma \leq u_{\alpha/2}) \\
=& \Phi(u_{\alpha/2}) - \Phi(-u_{\alpha/2}) \\
=& (1 - \alpha/2) - \alpha/2 \\
=& 1 - \alpha
\end{matrix}
$$

此式可改写为

$$
P(\bar{X} - \sigma u_{\alpha/2}/\sqrt{n} \leq \mu \leq \bar{X} + \sigma u_{\alpha/2}/\sqrt{n}) = 1 - \alpha
$$

此式指出

$$
[\bar{\theta}_1, \bar{\theta}_2] = [\bar{X} - \sigma u_{\alpha/2}/\sqrt{n}, \bar{X} + \sigma u_{\alpha/2}/\sqrt{n}]
$$

可作为 \\( \mu \\) 的区间估计, 置信系数为 \\( 1 - \alpha \\).

----
----

### 小样本法

**正态总体 \\( N(\mu, \sigma^2) \\) 中抽样本 \\( X_1, \cdots, X_n, \mu \\) 和 \\( \sigma^2 \\)都未知, 求 \\( \mu \\) 的区间估计.**

\\( \mu \\) 的点估计仍取为样本均值 \\( \bar{X} \\). 作为枢轴变量, 因 \\( \sigma \\) 未知

$$
\sqrt{n}(\bar{X} - \mu)/\sigma \nsim N(0, 1)
$$

把 \\( \sigma \\) 改为样本标准差 \\( S \\)

$$
\sqrt{n}(\bar{X} - \mu)/S \sim t_{n-1}
$$

\\( t \\) 分布密度关于 \\( 0 \\) 对称因而 \\( t\_{n-1}(1 - \alpha/2) = -t\_{n-1}(\alpha/2) \\), 得 \\( \mu \\) 的区间估计

$$
[\bar{X} - S t_{n-1}(\alpha/2)/\sqrt{n}, \bar{X} + S t_{n-1}(\alpha/2)/\sqrt{n}]
$$

置信系数为 \\( 1-\alpha \\). 称为"样本 \\( t \\) 区间估计".

**例**

估计一物件的重量 \\( \mu \\), 把它在天平上重复秤了 \\( 5 \\)次, 结果为

$$
5.52, 5.48, 5.64, 5.51, 5.43
$$

假定此天平无系统误差且随机误差服从正态分布. 则总体分布为 \\( N(\mu, \sigma^2) \\), \\( \mu, \sigma^2 \\) 都未知.

$$
\bar{X} = \frac{(5.52 + \cdots + 5.43)}{5} = 5.516
$$

$$
\begin{matrix}
S & = & \sqrt{\frac{1}{5-1}[(5.52-5.516)^2 + \cdots + (5.43-5.516)^2]} \\
  & = & \frac{1}{2}\sqrt{0.02412} \\
  & = & 0.078
\end{matrix}
$$

查表 \\( t_4(0.025) = 2.776 \\). 代入上式, 得 \\( \mu \\) 的置信系数 \\( 0.95 \\) 的区间估计为 \\( [5.419, 5.613] \\).

\\( [5.419, 5.613] \\) 是一个具体的区间, \\( \mu \\) 是一个虽然未知, 但其值确定的数. \\( [5.419, 5.613] \\) 这区间或者包含 \\( \mu \\), 或者不包含, 二者只居其一. 说这区间的置信系数为 \\( 0.95 \\), 其确切意义应当是: 它是根据所有的数据, 用一个其置信系数为 \\( 0.95 \\) 的方法做出的. 可见置信系数一次是针对方法: 用这个方法作出的区间估计, 平均 \\( 100 \\) 次中 \\( 95 \\) 次确包含所要估计的值. 一旦算出具体区间, 就不能再说它有 \\( 95% \\) 的机会包含要估计的值了. 这一点意义上的理解必须分清, 正如说一个人擅长挑西瓜: 他挑的瓜, 平均 \\( 100 \\) 个中 \\( 95 \\) 个好的. 某天他给你挑一个, 结果或好或坏, 必居其一, 不是 \\( 95% \\) 的好. 但是, 考虑到他挑瓜的技术, 我对他挑的比较放心, 这就是置信系数.

----

### 大样本法

**设某总体有均值 \\( \theta \\), 方差 \\( \sigma^2 \\). \\( \theta \\) 和 \\( \sigma^2 \\) 都未知, 从这总体中抽出样本 \\( X_1, \cdots, X_n \\), 要作 \\( \theta \\) 的区间估计.**

因为对总体分布没有作任何假定, 要作出满足条件 1-4 的枢轴变脸是不可能的. 但是, 若 \\( n \\) 相当大, 则据中心极限定理, 有 \\( \sqrt{n}(\bar{X} - \theta)/\sigma \sim N(0,1) \\). 但此处 \\( \sigma \\) 未知, 仍不能以 \\( \sqrt{n}(\bar{X} - \theta)/\sigma \\) 作为枢轴变量. 因为 \\( n \\) 相当大, 样本均方差 \\( S \\) 是 \\( \sigma \\) 的一个相合估计, 故可近似地用 \\( S \\) 代 \\( \sigma \\), 得

$$
\sqrt{n}(\bar{X} - \theta)/S \sim N(0,1)
$$

由此就不难得出 \\( \theta \\) 的区间估计

$$
[\bar{X} - S \mu_(\alpha/2)/\sqrt{n}, \bar{X} + S \mu_(\alpha/2)/\sqrt{n}]
$$

它的置信系数, 当 \\( n \\) 相当大时, 近似地为 \\( 1 - \alpha \\). 近似的程度如何不仅取决于 \\( n \\) 的大小, 还要看总体的分布如何.

----
----

"大样本方法": 基于有关变量的极限分布.

"小样本方法": 基于有关变量的确切分布.

基于有关变量的确切分布, 即使样本大小 \\( n = 10^10 \\),仍是小样本方法.
基友有关变量的极限分布, 即使样本大小 \\( n = 40 \\), 仍是大样本方法.

不言而喻, 大样本方法只有在样本大小较大时才宜于使用.

----
----

### 置信界

200

----
----

### 贝叶斯法

201

----
----