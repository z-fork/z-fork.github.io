---
title: Linear Algebra Note
layout: post
tags:
  - linear algebra
---


[第26集] 对称矩阵及正定性           

[第28集] 正定矩阵和最小值     
[第29集] 相似矩阵和若尔当形    
[第30集] 奇异值分解    
[第31集] 线性变换及对应矩阵    
[第32集] 基变换和图像压缩     

[第34集] 左右逆和伪逆

矩阵 * 向量: linear combine 列向量

a1 b1 c1   x     a1     b1     c1
a2 b2 c2 * y = x a2 + y b2 + z c2
a3 b3 c3   z     a3     b3     c3

向量 * 矩阵: linear combine 行向量

        a1 b1 c1
x y z * a2 b2 c2 = x a1 b1 c1 + y a2 b2 c2 + z a3 b3 c3
        a3 b3 c3

矩阵 * 矩阵: 矩阵消元

 1 0 0   A1   A1
-2 1 0 * B1 = B1-2A1
 0 0 1   C1   C1

 1 0 0
-2 1 0 = E_21 初等矩阵
 0 0 1

E_32(E_21 A) = U

寻找 E_?? A = U

(E_?? = E_32 E_21) A = U

不可逆, 奇异矩阵 可以 Ax = 0 线性组合找到 非 x=0 的解.

Gauss-Jordan

[A | I] - [I | A^-1]

E[A | I] = [I | ?]

EA = I, EI = E, ? = E = A^-1

---

E A = U

A = L * U
  = 下三角 * 上三角

2 1   1 0   2 1
8 7 = 4 1 * 0 3

      1 0   2 0   1 1/2
    = 4 1 * 0 3 * 0 1

    = 下三角 * 对角阵 * 上三角




向量空间, 为什么包括 0 向量?!

R^2 空间, x-y空间中, 空间满足, 向量加法 & 数乘 
  
  [3,2] * 0 = [0,0] = 0;
  [3,2] + [-3,-2] = [0,0] = 0;

  因为空间是闭包.

因此, 必须包含 0 向量!!

subspace.

R^2 subspace. 

 1, 自身,
 2, 通过原点的直线, 但与R^1 不同, 因为是两个分量, 仅仅是在一条直线上而已.
 3, 原点, 零向量


column space 列空间

ex:
  A = 1 3  columns in R^3
      2 3
      4 1

all their combinations form a subspace called column space C(A)

通过向量构成的空间. 他们仍在 R^3内.


Ax = b

a1 b1 c1   x1          a1      b1      c1
a2 b2 c2 * x2 = b = x1 a2 + x2 b2 + x3 c2
a3 b3 c3   x3          a3      b3      c3
a4 b4 c4               a4      b4      c4

即是否始终有解?, 转换为  a,b,c三个列向量是否可以表示所有 R^4. ( A = m*n = 4*3, m=4) 答案是 No!!

怎样的b 能让方程组有解??

只有当 b 属于 A的线性组合, 即在 A的列空间时 才有解!

Nullspace

Ax = 0, 的所有解构成的子空间.

R^3. ( A = m*n = 4*3, n=3)


rank of A



列满秩

m * n 

A = | 1 3 |   R = | 1 0 |
    | 2 1 |       | 0 1 |
    | 6 1 |       | 0 0 |
    | 5 1 |       | 0 0 |

rank A = r, r = n < m, 列满秩.

    r = | I |
        | 0 |

    未知数个数 < 方程数:
      线性相关的方程的b, 必须满足线性关系. 
        if r_3 = r_1 + r_2. 
        than b_3 = b_1 + b_2, 否则 没有解.

    线性无关 x.
    Nullspace 只有 0向量.
    m > n 表示. 方程数比未知数x多.
        2元一次方程. 即 n个方程, n个未知数. 且, 主元 = n 即 rank A = n. 列满秩

    0个或1个解!!

rank A = r, r = m < n, 行满秩.

    r = | I F |

    未知数个数 > 方程数:
      随意设置 (未知数个数 n - 方程数 r)个 任意值, 即 free variable. 解方程即可.
        free variable = n - rank A = n - r = n - m.

    无穷解!!

rank A = r, r = m = n,
    
    r = | I |

    唯一解!!

rank A = r, r < m & r < n.

    r = | I F |
        | 0 0 |

    无解!!

系数矩阵的秩 & 解关系! 如上.

基, 维数, 线性相关, 线性不相关

Independence,

除了 0 向量 不存在 Ax = 0, 则 线性不相关.

[v_1, v_2, ... v_n]x = 0

可逆..

rank(A) = #pivot columns = dimension of column space

---

把矩阵视为一种空间. 如同向量空间.

则, subspace是什么?

    上三角, 对称矩阵, 对角矩阵.


空间的延伸, R^n -> R^{n*n}

dim(S) + dim(U) = dim(S+U) - dim(S&U)

---

orthogonal!

subspace S is orthogonal to subspace T

means:
    every vector in S is orthogonal
    to ever vector in T.

---

Ax = b

A^{T}A \hat(x) = A^{T}b

---

Fibonacci : 0, 1, 1, 2, 3, 5, ...




---

特征值, 特征向量 的作用!!...

它决定着什么样的性质??..

---

