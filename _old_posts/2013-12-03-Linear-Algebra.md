---
title: Linear Algebra
layout: post
tags:
  - linear algebra
---

### 1. Basic Concepts and Notation (基本概念和符号)

For example

$$
\begin{matrix}
 4x_1 & - & 5x_2 & = & 13 \\ 
-2x_1 & + & 3x_2 & = & -9
\end{matrix}
$$

In matrix notation

$$ Ax = b $$

$$
with A = \begin{bmatrix}
 4 & -5 \\ 
-2 & 3
\end{bmatrix} ,
b = \begin{bmatrix}
13 \\ 
-9
\end{bmatrix}
$$

#### 1.1 Basic Notation (基本符号)

### 2. Matrix Multiplication (矩阵乘法)

#### 2.1 Vector-Vector Products (向量*向量)

#### 2.2 Matrix-Vector Products (矩阵*向量)

#### 2.3 Matrix-Matrix Products (矩阵*矩阵)

### 3. Operations and Properties (操作 & 属性)

#### 3.1 The Identity Matrix and Diagonal Matrices (单位矩阵, 对角阵)

#### 3.2 The Transpose (转置)

$$ A^T \in \mathbb{R}^{n \times m}, (A^T)_{ij} = A_{ji} $$

properties of transposes

$$
\begin{matrix}
\cdot & (A^T)^T & = & A \\
\cdot & (AB)^T  & = & B^{T}A^T \\
\cdot & (A+B)^T & = & A^T + B^T
\end{matrix}
$$

#### 3.3 Symmetric Matrices (对称矩阵)

#### 3.4 The Trace (迹)

#### 3.5 Norms (范数)

#### 3.6 Linear Independence and Rank (线性独立 & 秩)

#### 3.7 The Inverse (逆)

#### 3.8 Orthogonal Matrices (正交矩阵)

#### 3.9 Range and Nullspace of a Matrix (范围(列空间) & 零空间(核空间))

[Range](http://people.revoledu.com/kardi/tutorial/LinearAlgebra/MatrixRange.html)

    range (sometimes also called the columnspace)

    fuck , 头大.

    矩阵A的零空间是指方程组AX=0的解向量构成的空间，也就是AX=0的解空间。

    矩阵的列空间是指矩阵的列向量组构成的空间，也就是将列向量组的极大线性无关组找出来，然后做线性组合而生成的所有向量构成的空间。

    线性变换的值域与核

    值域和核

    核就是以这个矩阵为系数矩阵的齐次方程组的解集

    值域就是先找出上述方程的解集的基

    然后找出包含这组基的线性空间的基

    然后在线性空间的基里面去除解集的基，剩下的就是值域的基

##### Range

A linear system \\(Ax=b\\) can be thought as a linear transformation of a vector \\(x\\) in \\(n\\) dimensional space into vector \\(b\\) in \\(m\\) dimensional space. The solution space \\(x\\) of a non-homogeneous system of equations \\(Ax=b\\) is called the range of matrix \\(A\\) and the dimension of the range of matrix \\(A\\) is called the rank of \\(A\\) symbolizes as \\(\rho(A)\\).

The dimension of the range and the null space of a matrix are related through fundamental relationship \\(rank(A)+nullity(A)=dim(A)=n\\). where \\(n\\) is the number of original unknowns.

##### Nullspace

一个算子 \\(A\\) 的 零空间 是方程 \\(Av=0\\) 的所有解 \\(v\\) 的集合. 它也叫做 \\(A\\)的 核, 核空间.

用 集合建造符号 表示为

$$ Null(A) = \{ v \in V: Av = 0 \} $$

如果算子是在向量空间上的线性变换(线性映射), 零空间就是线性子空间. 因此零空间是线性空间(向量空间).

##### 线性空间(向量空间)

向量空的一种例子是齐次线性方程组 (常数项都是 0 的线性方程组) 的解的集合.

例如下面的方程组

$$
\begin{matrix}
 3x & + & 2y & - &  z & = & 0 \\ 
  x & + & 5y & + & 2z & = & 0
\end{matrix}
$$

如果 \\((x_1, y_1, z_1)\\) 和 \\((x_2, y_2, z_2)\\) 都是解, 那么它们的"和" \\((x_1+x_2, y_1+y_2, z_1+z_2)\\) 也是一组解.

同样, 将一组解乘以一个常数后, 仍然会是一组解.

因此这个方程组的所有解组成一个向量空间.(这样定义的"向量加法"和"标量乘法"满足向量空间的公理)

##### 线性子空间

如果一个向量空间 \\(V\\) 的一个非空子集合 \\(W\\) 对于 \\(V\\) 的加法及标量乘法都封闭(也就是说任意 \\(W\\) 中的元素相加或者和标量相乘之后仍然在\\(W\\)之中), 那么将 \\(W\\) 称为 \\(V\\) 的线性子空间.

\\(V\\) 的子空间中, 最平凡的就是空间 \\(V\\) 自己, 以及只包含 \\(0\\) 的子空间 \\(O\\).

##### 基底

给出一个向量集合\\(B\\), 包含它的最小子空间就称为它的生成子空间, 也称线性包络, 记作 \\(span(B)\\)

给出一个向量集合\\(B\\), 若它的生成集就是向量空间\\(V\\), 则称\\(B\\)为\\(V\\)的一个生成集.

如果一个向量空间\\(V\\)拥有一个元素个数有限的生成集, 那么就称\\(V\\)是一个有限维空间.

可以生成一个向量空间\\(V\\)的线性无关子集, 称为这个空间的基.

对非零向量空间\\(V\\), 基是\\(V\\)"最小"生成集.

向量空间的基提供了一个坐标系.

一个向量空间的所有基都拥有相同基数, 称为该空间的维度.

任何一组基中的元素个数都是定值, 等于空间的维度.

例如, 各种实数向量空间

$$ \mathbb{R}^0, \mathbb{R}^1, \mathbb{R}^2, \mathbb{R}^3, \cdots, \mathbb{R}^{\infty}, \cdots $$

\\( \mathbb{R}^n \\) 的维度就是 \\(n\\). 在一个有限维的向量空间(维度是 \\(n\\))中, 确定一组基 \\(B = \\{e_1, e_2, \cdots, e_n\\} \\), 那么所有的向量都可以用 \\(n\\)个标量来表示. 比如说, 如果某个向量\\(v\\)表示为

$$ v = \lambda_{1}e_1 + \lambda_{2}e_2 + \cdots + \lambda_{n}e_n $$

那么 \\(v\\)可以用数组 \\( v = (\lambda_1, \lambda_2, \cdots, \lambda_n) \\)来表示. 这种表示方式称为向量的坐标表示. 按照这种表示方法, 基中元素表示为

$$
\begin{matrix}
 e_1 & = & ( & 1, &  0, & \cdots, & 0 & ) \\ 
 e_2 & = & ( & 0, &  1, & \cdots, & 0 & ) \\
 e_n & = & ( & 0, &  0, & \cdots, & 1 & )
\end{matrix}
$$

#### 3.10 The Determinant (行列式)

#### 3.11 Quadratic Forms and Positive Semidefinite Matrices (二次型 & 半正定矩阵)

#### 3.12 Eigenvalues and Eigenvectors (特征值 & 特征向量)

#### 3.13 Eigenvalues and Eigenvectors of Symmetric Matrices (特征值 & 对称矩阵的特征向量)

### 4. Matrix Calculus (矩阵演算)

#### 4.1 The Gradient (梯度)

#### 4.2 The Hessian (??)

#### 4.3 Gradients and Hessians of Quadratic and Linear Functions (??)

#### 4.4 Least Squares (最小二乘)

#### 4.5 Gradients of the Determinant (行列式的梯度)

#### 4.6 Eigenvalues as Optimization (优化特征值)

