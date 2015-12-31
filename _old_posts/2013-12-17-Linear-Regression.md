---
title: Linear Regreesion
layout: post
tags:
  - linear regression

---

* 1 [线性回归问题](#1)
* 2 [线性回归求解](#2)
  * 2.1 [选择损失函数](#2.1)
  * 2.2 [求解回归参数](#2.2)
    * 2.2.1 [梯度下降法](#2.2.1)
    * 2.2.2 [最小二乘法](#2.2.2)
    * 2.2.3 [参数的一些性质](#2.2.3)
  * 2.3 [置信区间与预测](#2.3)
  * 2.4 [假设检验问题](#2.4)
* 3 [优化](#3)
  * 3.1 [拟合](#3.1)
    * 3.1.1 [合适的拟合](#3.1.1)
    * 3.1.2 [欠拟合](#3.1.2)
    * 3.1.3 [过拟合](#3.1.3)
  * 3.2 [子集选择(减少特征)](#3.2)
  * 3.3 [正则化](#3.3)
    * 3.3.1 [L1范数](#3.3.1)
    * 3.3.2 [L2范数](#3.3.2)

----
----

<h4 id="1">1 线性回归问题</h4>

有这样的一个问题, 已知一些住房的当前市场价值和影响价值的\\(r = 4\\)个因素, 居住面积, 位置, 去年的评估价值, 建筑质量. 求各个因素所占的比重, 并预测其他住房当前市场价.

\\( Y \\) = 住房的当前市场价值

\\( z_1 \\) = 居住面积( 平方英尺 )

\\( z_2 \\) = 位置( 城市区域的指标 )

\\( z_3 \\) = 去年的评估价值

\\( z_4 \\) = 建筑质量( 每平方英尺价格 )

本文讨论, 所有影响价值的因素是线性的, 可以建立线性回归方程.

**真实的回归模型:**

$$
Y = Z \beta + \epsilon
$$

**估计的回归模型:**

$$
\begin{matrix}
Y      & = & \hat{\beta_0} + \hat{\beta_{1}}z_1 + \cdots + \hat{\beta_{r}}z_{r} & + & \hat{\epsilon} \\ 
(响应) & = & [均值(依赖于 z_1,\cdots,z_r)]                                      & + & (残差)
\end{matrix}
$$

用矩阵形式表示:

$$
\begin{matrix}
\begin{bmatrix}  
Y_1\\
Y_2\\
\cdot\\
\cdot\\
\cdot\\
Y_n
\end{bmatrix} & = & 
\begin{bmatrix}                                          
1     & z_{11} & z_{12} & \cdot & \cdot & \cdot & z_{1r} \\ 
1     & z_{21} & z_{22} & \cdot & \cdot & \cdot & z_{2r} \\ 
\cdot & \cdot  & \cdot  & \cdot &       &       & \cdot  \\ 
\cdot & \cdot  & \cdot  &       & \cdot &       & \cdot  \\ 
\cdot & \cdot  & \cdot  &       &       & \cdot & \cdot  \\ 
1     & z_{n1} & z_{n2} & \cdot & \cdot & \cdot & z_{nr} \\ 
\end{bmatrix} & 
\begin{bmatrix} 
\hat{\beta_0}\\       
\hat{\beta_1}\\       
\cdot\\         
\cdot\\         
\cdot\\         
\hat{\beta_r}         
\end{bmatrix} & + & 
\begin{bmatrix}
\hat{\epsilon_1}\\     
\hat{\epsilon_2}\\     
\cdot\\        
\cdot\\        
\cdot\\        
\hat{\epsilon_n}       
\end{bmatrix}  \\
Y & = & Z & \hat{\beta} & + & \hat{\epsilon}
\end{matrix}
$$

a. 当方程个数小于未知量个数时, 转化成求解**不定方程组**的问题.

使用矩阵描述, \\( n < r = rank(\beta) \\), 有无穷多解.

方法: 增广矩阵

b. 当方程个数等于未知量个数时, 转化成求解**恰定方程组**的问题.

使用矩阵描述, \\( n = r = rank(\beta) \\), 有唯一解.

方法: 增广矩阵

c. 当方程个数大于未知量个数时, 转化成求解**超定方程组**的问题.

使用矩阵描述, \\( n > r = rank(\beta) \\), 求近似解.

方法: **松弛求解** - 求解问题改变为**求最小误差**的问题, 就是在无法完全满足给定的这些条件的情况下, 求一个近似的解.

----
----

<h4 id="2">2 线性回归求解</h4>

**linear regression 求解的是上方描述中的c类问题, 即松弛求解.**

回归模型中误差项假定具有以下性质:

1. \\( E(\epsilon_j) = 0 \\).
2. \\( Var(\epsilon_j) = \sigma^2(常数) \\).
3. \\( Cov(\epsilon_j, \epsilon_k) = 0, j \neq k \\).

关于误差项的假定很宽松, 稍后我们将增添一个**正态性假定**, 以便能构造**置信区间**和进行**假设检验**.

----

<h4 id="2.1">2.1 选择损失函数</h4>

有关[损失函数](url)请看这里.

求最小误差 -- 损失函数(loss function)

线性回归, 采用的是平方损失函数:

$$
L(Y, f(x)) = (Y - f(x))^2
$$

为了计算方便, 会乘以常数1/2:

$$
J(\beta) = \frac{1}{2} \sum_{i=1}^{n} (Z_i \beta - Y_i)^2
$$

$$
min J_{\beta}
$$

----

<h4 id="2.2">2.2 求解回归参数</h4>

我们确定于数据相容的**回归系数** \\( \beta \\) 和 **误差方差** \\( \epsilon^2 \\) 的值.

设 \\( \beta \\) 的一个尝试值, 使损失函数极小, 称为 \\( \beta \\) 的**最小二乘估计**, 记为 \\( \hat{ \beta } \\).

偏差

$$
\hat{\epsilon} = Y - Z \hat{ \beta } 
$$

称为**残差**.

设 \\( Z \\) 有**满秩**, 且 \\( r+1 \leq n \\), \\( \beta \\) 的最小二乘估计为

$$
\hat{ \beta } = (Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T}Y
$$

接下来介绍求模型参数的方法.

----

<h4 id="2.2.1">2.2.1 梯度下降法</h4>

数值分析思路, 迭代求近似解.

----

<h4 id="2.2.2">2.2.2 最小二乘法</h4>

线性代数 & 微积分, 求确切值.

**矩阵投影**

![](/files/2013/LinearR_2-2-2.png)

\\( Y = \hat{ Y } + \hat{\epsilon} = Z \hat{\beta} + \hat{\epsilon} \\).

\\( \hat{Y} \\) 为拟合值, 在 \\( Z \\) 的Column Space 组成的平面中.

\\( \hat{\epsilon} \\) 为残差向量, \\( Y \\) 点到 \\( Z \\) 的Column Space 组成的平面的距离, 当且仅当垂直时距离最小.

即 \\( Y \\) 可以分为 \\( Z \\) 的 Column Space 中投影 \\(\hat{Y}\\) + \\( Z \\) 的 Null Space 中的投影 \\( \hat{\epsilon} \\).

已知两个空间正交(垂直).

$$
\begin{matrix}
Z^\mathrm{T} \hat{\epsilon}        & = & 0 \\ 
Z^\mathrm{T} ( Y - \hat{Y} )       & = & 0 \\ 
Z^\mathrm{T} ( Y - Z \hat{\beta} ) & = & 0 \\
Z^\mathrm{T} Z \hat{\beta}         & = & Z^\mathrm{T} Y
\end{matrix}
$$

又假设 \\( Z_i \\) 线性无关, 即 \\( Z \\) 满秩

$$
\hat{ \beta } = ( Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T} Y
$$

证毕.

**矩阵求导计算**

以微积分的思路考虑 

最小二乘估计是以残差平方和极小, 转化为二次函数求极值问题, 又因二次函数为凸函数, 对 \\( \beta \\) 求导, 导数为零.

\\(S( \hat{\beta} ) = \frac{1}{2}(Y - Z \hat{\beta})^\mathrm{T}(Y - Z \hat{\beta})\\) 对 \\(\hat{\beta}\\) 求导

$$
\begin{matrix}
  & \bigtriangledown_{ \hat{\beta} } S( \hat{ \beta }) \\
= & \bigtriangledown_{ \hat{\beta} } \frac{1}{2}(Y - Z\hat{\beta})^\mathrm{T}(Y - Z\hat{\beta}) \\ 
= & Z^\mathrm{T}Z\hat{\beta} - Z^\mathrm{T}Y \\
= & 0
\end{matrix}
$$

又因为 \\( Z \\) 满秩

$$
\hat{ \beta } = ( Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T}Y
$$

证毕.

2. 设 \\( \hat{ Y } = Z \hat{ \beta } = HY \\) 为 \\( Y \\) 的 **拟合值**, 其中 \\( H = Z( Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T} \\) 称为 **冒矩阵**, 此时残差
$$
\hat{ \epsilon } = Y - \hat{Y} = [ I - Z( Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T} ]Y = (I - H)Y = (I - H)(Z \beta + \epsilon) = (I - H)\epsilon
$$

**说明**

\\( H = Z(Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T} \\) 是将 \\( Y \\) 投影到 \\( Z \\) Column space 的投影矩阵

\\( [ I - Z( Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T} ] \\) 是将 \\( Y \\) 投影到 \\( Z \\) Null space 的投影矩阵

由投影矩阵的性质可知

1. \\( H^\mathrm{T} = H \\)
2. \\( H^2 = H \\)

同理

1. \\( [I - H]^\mathrm{T} = [I - H] \\)
2. \\( [I - H]^2 = [I - H] \\)

由 Column space 与 Null space 关系可知

\\( Z^\mathrm{T}[I - H] = 0 \\)

> **附 1**  
若 \\( Z \\) 不满秩, 根据**广义逆** \\( (Z^\mathrm{T}Z)^{-} \\) 对 \\( Z( Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T} \\) 均成立

----

<h4 id="2.2.3">2.2.3 参数的一些性质</h4>

**参数的方差**

**1. 估计量 \\( \hat{ \beta } \\), 估计量的方差估计**

$$
\begin{matrix}
\hat{ \beta } & = & (Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T}Y \\
              & = & (Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T}(Z\beta + \epsilon) \\
              & = & \beta + (Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T}\epsilon
\end{matrix}
$$

$$
\begin{matrix}
Var(\hat{\beta}) & = & E[(\hat{\beta} - E(\hat{\beta}))^2] \\
                 & = & E[(\hat{\beta} - \beta)(\hat{\beta} - \beta)^\mathrm{T}] \\
                 & = & E{[(Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T}\epsilon] \ [(Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T}\epsilon]^\mathrm{T}} \\
                 & = & (Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T}E(\epsilon\epsilon^\mathrm{T})Z(Z^\mathrm{T}Z)^\mathrm{-1} \\
                 & = & \sigma^2 (Z^\mathrm{T}Z)^\mathrm{-1}
\end{matrix}
$$

**2. 残差 \\( \epsilon \\), 残差平方的无偏估计**

$$
\hat{\epsilon} = (I - H)\epsilon
$$

$$
\hat{ \epsilon }^\mathrm{T}\hat{\epsilon} = \epsilon^\mathrm{T}(I - H)\epsilon
$$

求期望, **由于残差平方和是标量(Scalar), 可以采用迹(Trace):!!!** \\( tr(AB) = tr(BA) \\)

$$
\begin{matrix}
  & E[\hat{ \epsilon }^\mathrm{T}\hat{\epsilon}] \\
= & E[\epsilon^\mathrm{T}(I - H)\epsilon] \\
= & E{tr[\epsilon^\mathrm{T}(I - H)\epsilon]} \\
= & E{tr[(I - H)\epsilon\epsilon^\mathrm{T}]} \\
= & tr[(I - Z(Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T}) E(\epsilon\epsilon^\mathrm{T}) \\
= & \sigma^2 [tr(I) - tr(Z(Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T})] \\
= & \sigma^2 [tr(I) - tr((Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T}Z)] \\
= & \sigma^2 [n - tr(I_{r+1})] \\
= & \sigma^2 (n-r-1)
\end{matrix}
$$

即

$$
E(\hat{ \epsilon }^\mathrm{T}\hat{ \epsilon }) = (n-r-1)\sigma^2
$$

证明了\\( \hat{\sigma}^2 \\) 为 \\( \sigma^2 \\) 的无偏估计.

当 \\( \epsilon \\) 服从正态分布 \\( N(0, \sigma^2) \\) 时, 有

$$
(n-r-1)\hat{\sigma}^2 \sim \sigma^2 \chi_{n-r-1}^2
$$

具有 \\( n-r-1 \\) 自由度的 \\( \chi^2 \\)分布.

----

**最小二乘估计 BLUE ( Best Linear Unbiased Estimators ) 最优线性无偏估计量**

> **高斯—马尔科夫假设**  
1. \\( E(\epsilon) = 0 \\), 残差具有零均值.  
2. \\( Var(\epsilon) = \sigma^2 < \infty \\), 残差具有常数方差, 且有限.  
3. \\( cov(\epsilon_i, \epsilon_j) = 0 \\), 残差之间相互独立.  
4. \\( cov(\epsilon, Z) = 0 \\), 残差项与变量 \\( Z \\) 无关.  
5. \\( \epsilon \sim N(0, \sigma^2) \\), 残差项服从正态分布.

----

> \\( \hat{\epsilon} \\) 正态分布 ---[ \\( Y \\) 是 \\( \hat{\epsilon} \\) 的线性函数 ]---> \\( Y \\) 正态分布 ---> [ \\( \hat{\beta} \\) 是 \\( Y \\) 的线性函数 ] ---> \\( \hat{\beta} \\) 正态分布.

**1. Linear( 线性 )**

证明: \\( \hat{ \beta } \\) 是 \\( Y \\) 的线性函数.

代数, 投影角度

$$
\hat{ \beta } = (Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T}Y
$$

\\( (Z^\mathrm{T}Z)^\mathrm{-1} \\) 是常数, \\( Z^\mathrm{T} \\) 是变换矩阵.

证毕.

**2. Unbiased( 无偏估计量 )**

证明: \\( E(\hat{ \beta }) = \beta \\).

$$
\begin{matrix}
E(\hat{ \beta }) & = & E((Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T}Y) \\
                 & = & E((Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T}(Z \beta + \epsilon)) \\
                 & = & E((Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T}Z \beta + (Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T} \epsilon) \\
                 & = & E(\beta + (Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T} \epsilon) \\
                 & = & E(\beta) + (Z^\mathrm{T}Z)^\mathrm{-1}E(Z^\mathrm{T} \epsilon) \\
                 & = & \beta
\end{matrix}
$$

因为 \\( E(\epsilon) = 0 \\), 所以 \\( (Z^\mathrm{T}Z)^\mathrm{-1}E(Z^\mathrm{T} \epsilon) = 0 \\).

证毕.

**3. Best( 最优 )**

证明: 所有线性无偏估计量里, \\( \hat{ \beta } \\) 具有最小方差的估计量.

> 思路: 证明对任一线性无偏估计量 \\( \dot{\beta} \\), \\( Var(\dot{\beta}) \geq Var(\hat{ \beta }) \\)

计算 \\( Var(\dot{ \beta }) \\)

$$
\begin{matrix}
Var(\dot{ \beta }) & = & Var(\dot{ \beta } - \hat{ \beta } + \hat{ \beta }) \\
                   & = & Var(\dot{ \beta } - \hat{ \beta }) + Var(\hat{ \beta }) + Cov(\dot{ \beta } - \hat{ \beta }, \hat{ \beta })
\end{matrix}
$$

已知 \\( Var(x) \geq 0 \\), 且 \\( \dot{ \beta }, \hat{ \beta } \\), 只要能证明 \\( Cov(\dot{ \beta } - \hat{ \beta }, \hat{ \beta }) = 0 \\) 即可.

因为 \\( \dot{\beta} \\) 是线性, 满足

$$
\begin{matrix}
\dot{\beta} & = & AY \\
            & = & A(Z\beta + \epsilon) \\
            & = & AZ\beta + A\epsilon
\end{matrix}
$$

因为 \\( \dot{\beta} \\) 是无偏估计量, 满足 \\( E(\dot{\beta}) = \beta \\)

$$
AZ = I
$$

同理

\\( \hat{ \beta } = BY \\) & \\( BZ = I \\).

计算得

$$
\dot{ \beta } - \hat{ \beta } = AY - BY = (A-B)(Z\beta + \epsilon) = (A-B)\epsilon
$$

$$
(A - B)Z = I - I = 0
$$

因此

$$
\begin{matrix}
  & Cov(\dot{ \beta } - \hat{ \beta }, \hat{ \beta }) \\
= & E\{[\dot{ \beta } - \hat{ \beta } - E(\dot{ \beta } - \hat{ \beta })]\ [\hat{ \beta } - E(\hat{ \beta })]^\mathrm{T} \} \\
= & E[(\dot{ \beta } - \hat{ \beta }) (\hat{ \beta } - \beta)^\mathrm{T}] \\
= & E[((A-B)\epsilon) ((Z^\mathrm{T}Z)^\mathrm{-1}Z^\mathrm{T}\epsilon)^\mathrm{T}] \\
= & E[((A-B)\epsilon) (\epsilon^\mathrm{T}Z(Z^\mathrm{T}Z)^\mathrm{-1})] \\
= & (Z^\mathrm{T}Z)^\mathrm{-1}[(A-B)Z]E(\epsilon\epsilon^\mathrm{T}) \\
= & 0
\end{matrix}
$$

证毕.

----

<h4 id="2.3">2.3 置信区间与预测</h4>

----

<h4 id="2.4">2.4 假设检验问题</h4>

----
----

<h4 id="3">3 优化</h4>

----

<h4 id="3.1">3.1 拟合</h4>

<h4 id="3.1.1">3.1.1 合适的拟合</h4>

![](/files/2013/LinearR_3-1-1.png)

----

<h4 id="3.1.2">3.1.2 欠拟合</h4>

![](/files/2013/LinearR_3-1-2.png)

----

<h4 id="3.1.3">3.1.3 过拟合</h4>

![](/files/2013/LinearR_3-1-3.png)

----

<h4 id="3.2">3.2 子集选择(减少特征)</h4>

----

<h4 id="3.3">3.3 正则化</h4>

----

<h4 id="3.3.1">3.3.1 L1范数</h4>

----

<h4 id="3.3.2">3.3.2 L2范数</h4>

----
