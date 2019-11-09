---
title: "[Linear Algebra]Cramer's Rule"
layout: post
date: 2019-11-09 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Graphics
- Linear Algebra
- Cramer's Rule
category: blog
author: heetaekim
description: Cramer's Rule
MathJax: true
---
# Cramer's Rule
----
Linear System을 푸는 방법은 대치법, 가우스 소거법, 크래머 공식 등이 있는데 그 중 하나인 Cramer's Rule에 대해 소개한다. Cramer's Rule은 유일한 해를 갖고 변수와 방정식 수가 같은 연립방정식을 갖는, 즉, Sqare Matrix인 Linear System을 푸는 공식이다.

&nbsp;&nbsp;&nbsp;&nbsp;

### 2 by 2 Matrix 예제
$$
a_{1,1}x_{1} + a_{1,2}x_{2} = c_1 \\
a_{2,1}x_{1} + a_{2,2}x_{2} = c_2
$$

위 식의 해를 구하면 다음과 같다.

$a_{1,1}a_{2,2} - a_{1,2}a_{2,1} \neq 0$ 일 때,
$$
x_1 = 
\frac
{c_1a_{2,2} - c_2a_{1,2}}
{a_{1,1}a_{2,2} - a_{1,2}a_{2,1}}
,

x_2 = 
\frac
{a_{1,1}c_2 - a_{2,1}c_1}
{a_{1,1}a_{2,2} - a_{2,1}a_{1,2}}
$$

이를 아래와 같이 determinant로 표현할 수 있다.

$$
x_1 = 
\frac
{\begin{vmatrix} c_{1} & a_{1,2} \ c_{2} & a_{2,2} \end{vmatrix}} 
{\begin{vmatrix} a_{1,1} & a_{1,2} \ a_{2,1} & a_{2,2} \end{vmatrix}}
,

x_2 = 
\frac
{\begin{vmatrix} a_{1,1} & c_{1} \ a_{2,1} & c_{2} \end{vmatrix}} 
{\begin{vmatrix} a_{1,1} & a_{1,2} \ a_{2,1} & a_{2,2} \end{vmatrix}}
$$

위 공식을 통해 아래 Matrix를 풀어보자!

$$
3x + 2y = 6 \\
x - y = 1
$$

의 해는 아래와 같다.

$$
x_1 = 
\frac
{\begin{vmatrix} 6 & 2 \ 1 & -1 \end{vmatrix}} 
{\begin{vmatrix} 3 & 2 \ 1 & -1 \end{vmatrix}}
 = 
\frac
{8}
{5}
,

x_2 = 
\frac
{\begin{vmatrix} 3 & 6 \ 1 & 1 \end{vmatrix}} 
{\begin{vmatrix} 3 & 2 \ 1 & -1 \end{vmatrix}}
 = 
\frac
{3}
{5}
$$

&nbsp;&nbsp;&nbsp;&nbsp;

### General Form
위 방법을 사용해서 $N \times N$ Matrix의 해를 일반화하면 아래와 같다.

아래와 같은 Linear System이 있다고 가정하자

$$
 \begin{bmatrix} 
 a_{1,1}  &  a_{1,2}  & \dots  & a_{1,n} \\
 a_{2,1}  &  a_{2,2}  & \dots  & a_{2,n} \\ 
 \vdots   &  \vdots   & \ddots & \vdots \\ 
 a_{n,1}  &  a_{n,2}  & \dots  & a_{n,n} \\ 
 \end{bmatrix} 
 x = 
 \begin{bmatrix} 
 b_{1}  \\
 b_{2}  \\ 
 \vdots   \\ 
 b_{n}  \\ 
 \end{bmatrix} 
 $$

이 때 $A_{j}$를 $A$의 $j$번째 열을 $B$로 대신해 얻는 행렬이라고 정의한다. 아래와 같다.

$$
 \begin{bmatrix} 
 a_{1,1}  &  \dots & b_1  & \dots  & a_{1,n} \\
 a_{2,1}  &  \dots & b_2  & \dots  & a_{2,n} \\
 \vdots  &  \hfill & \vdots  & \hfill  & \vdots \\
 a_{n,1}  &  \dots & b_n  & \dots  & a_{n,n} \\ 
 \end{bmatrix} 
$$

여기서 해 $x_{j}$는 다음과 같다.

$$
x_{j} =
\frac
{det A_{j}}
{det A} =

\frac
{ \begin{bmatrix} 
 a_{1,1}  &  \dots & b_1  & \dots  & a_{1,n} \\
 a_{2,1}  &  \dots & b_2  & \dots  & a_{2,n} \\
 \vdots  &  \hfill & \vdots  & \hfill  & \vdots \\
 a_{n,1}  &  \dots & b_n  & \dots  & a_{n,n} \\ 
 \end{bmatrix} }
{ \begin{bmatrix} 
 a_{1,1}  &  \dots & a_{1,j}  & \dots  & a_{1,n} \\
 a_{2,1}  &  \dots & a_{2,j}  & \dots  & a_{2,n} \\
 \vdots  &  \hfill & \vdots  & \hfill  & \vdots \\
 a_{n,1}  &  \dots & a_{n,j}  & \dots  & a_{n,n} \\ 
 \end{bmatrix} }

 (j = 1, \dots , n)
$$

&nbsp;&nbsp;&nbsp;&nbsp;

# Cramer's Rule의 한계
----
Cramer's Rule은 $n \times n$의 matrix에서 Linear System의 해를 구할 때 Gaussian elimination은 $O(n^3)$ 이고 Cramer’s
rule은 $O(n!)$ 이다. 그렇기 때문에 n이 커질수록 비효율적이게 된다.