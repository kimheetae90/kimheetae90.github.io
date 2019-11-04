---
title: "[Linear Algebra]Square Matrix의 종류"
layout: post
date: 2019-11-04 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Graphics
- Linear Algebra
- Square Matrix
category: blog
author: heetaekim
description: Square Matrix 종류
---
# Square Matrix
----

$$
 A = 
 \begin{bmatrix} 
 a_{1,1}  &  a_{1,2}  & \dots  & a_{1,n} \\
 a_{2,1}  &  a_{2,2}  & \dots  & a_{2,n} \\ 
 \vdots   &  \vdots   & \ddots & \vdots \\ 
 a_{n,1}  &  a_{n,2}  & \dots  & a_{n,n} \\ 
 \end{bmatrix} 
 $$

Square Matrix는 N by N Matrix, 즉 행과 열의 갯수가 같은 Matrix로 선형대수에 있어서 중요한 요소이다. 왜냐하면 Linear System을 표현하거나 다루는 과정 혹은 해를 구할 때 주요 부분을 차지하기 때문이다. Graphics에서는 Square Matrix를 통해 Linear Transform 등 기하 정보를 표현한다. 이 포스팅에서는 몇가지 종류의 Square Matrix에 대해 알아보도록 하자.

&nbsp;&nbsp;&nbsp;&nbsp;
## Diagonal Matrix
----
Diagonal Matrix는 Diagonal(주대각선)을 제외한 나머지 element들은 0인 Matrix를 지칭한다. 이 떄 Diagonal은 0일수도, 아닐수도 있다. 

$$
 A = 
 \begin{bmatrix} 
 a_{1,1}  &  0  & \dots  & 0 \\
 0  &  a_{2,2}  & \dots  & 0 \\ 
 \vdots   &  \vdots   & \ddots & \vdots \\ 
 0  &  0  & \dots  & a_{n,n} \\ 
 \end{bmatrix} 
 $$

Diagonal Matrix의 특징은 다음과 같다.

**1. A와 B가 Diagonal Matrix라면 $C = AB$인 Matrix C는 Diagonal이다.**

**2. A와 B가 Diagonal Matrix라면 $C = AB = BA$이다. 즉, 교환법칙이 성립한다.**

**3. A가 Diagonal Matirx이고 B는 일반 Matrix라면 $C = AB$ 의 i번째 행은 B의 i번째 행에 $a_{i,i}$ 요소를 곱한 것 과 같다. 열을 계산할 때도 같다.**


&nbsp;&nbsp;&nbsp;&nbsp;

### Scalar Matrix
Diagonal Matrix중 Diagonal이 모두 같은 Matrix이다.

&nbsp;&nbsp;&nbsp;&nbsp;

### Identity Matrix
Scalar Matrix 중 Diagonal이 모두 1인 Matrix이다. 단위행렬이라고 부른다. $I$라고 표기하고 Matrix 곱의 항등원이 된다. 즉, $MI = IM = M$ 이다.

&nbsp;&nbsp;&nbsp;&nbsp;

## Triangular Matrix
----
Diagonal을 기준으로 아래 모든 요소가 0인 Matrix를 Upper Triangular Matrix라고 하고 아래가 모두 0인 Matrix를 Lower Triangular Matrix라고 한다. 그리고 여기서 Diagonal까지 0인 Matrix를 Strict Triangular Matrix라고 한다.

$$
 \begin{bmatrix} 
 a_{1,1}  &  a_{1,2}  & \dots  & a_{1,n} \\
 0  &  a_{2,2}  & \dots  & a_{2,n} \\ 
 \vdots   &  \vdots   & \ddots & \vdots \\ 
 0  &  0  & \dots  & a_{n,n} \\ 
 \end{bmatrix} 
 $$

_Upper Triangular Matrix_

 $$
 \begin{bmatrix} 
 a_{1,1}  &  0  & \dots  & 0 \\
 a_{2,1}  &  a_{2,2}  & \dots  & 0 \\ 
 \vdots   &  \vdots   & \ddots & \vdots \\ 
 a_{n,1}  &  a_{n,2}  & \dots  & a_{n,n} \\ 
 \end{bmatrix} 
 $$

_Lower Triangular Matrix_

$$
 \begin{bmatrix} 
 0  &  a_{1,2}  & \dots  & a_{1,n} \\
 0  &  0  & \dots  & a_{2,n} \\ 
 \vdots   &  \vdots   & \ddots & \vdots \\ 
 0  &  0  & \dots  & 0 \\ 
 \end{bmatrix} 
 $$

_Strict Triangular Matrix_

Triangular Matrix의 성질은 다음과 같다.

**1. A와 B가 Lower Triangular Matrix라면 $C = AB$는 Lower Triangular Matrix이고 Upper인 경우도 마찬가지이다.**

**2. A와 B가 Lower Triangular Matrix라면 $C = A + B$는 Lower Triangular Matrix이고 Upper인 경우도 마찬가지이다.**

**3. A가 Invertible한 Lower Triangular Matrix라면 A의 역행렬도 Lower Triangular Matrix이고 Upper인 경우도 마찬가지이다.**

**4. Triangular Matrix의 Determinant는 Diagonal의 곱이다.**

일반 Matrix를 Triangular Matrix들의 곱으로 나타내는 방법을 LU Decomposition이라고 한다.

&nbsp;&nbsp;&nbsp;&nbsp;

# Determinant
----
Square Matrix에 수를 대응시키는 방식이고 행렬식이라고 부른다. Determinant가 0인지 아닌지는 연립방정식에서는 유일 해를 갖는지 갖지 않는지를 판별하는 중요한 수가 된다.

행렬식은 라플라스 전개를 통해 정의할 수 있다.

![라플라스 전개](/assets/images/post/2019-11-04-Square-Matrix/Determinant.jpg)

이 식에서 알아야 할 용어는 submatrix, minor, cofactor이다. cofactor는 기준이 되는 element이다. 위 식에서 cij가 된다. submatrix는 cofactor에 해당되는 행과 열을 삭제한 Matrix이다. 이 submatrix의 determinant를 minor라고 한다. 위와 같은 공식을 재귀적으로 계산하면 Determinent를 구할 수 있다.

Determinant의 특징은 아래와 같다.

![Square Matrix Example](/assets/images/post/2019-11-04-Square-Matrix/SquareMatrix.jpg)

&nbsp;&nbsp;&nbsp;&nbsp;

# Inverse
----
M1M2 = I 일 때, M1와 M2는 Invertible하다고 한다. 이 때 각각은 서로의 Inverse Matrix, 역행렬이라고 부르고 (M1-1)과 같이 표기한다.

Inverse Matrix를 구하는 방법은 가우스 소거법, Adjoint를 활용해 구하는 방법, Cramer 공식이 있다.

&nbsp;&nbsp;&nbsp;&nbsp;

### Inverse Matrix의 특징
1. MM-1 = I 이면 M-1M = I
2. (M1M2)-1 = M2-1M1-1
3. (m-1)-1 = M
4. (aM)-1 = (1/a)M-1 (a가 0이 아닐때)


### Inverse Matrix의 조건
1. rank가 N
2. nonsingular
3. determinant != 0
4. 행과 열이 Linearly independent
5. Considered as a transformation, it does not reduce dimensionality.


&nbsp;&nbsp;&nbsp;&nbsp;

# Singular Matrix
----
Invertible한 Matrix를 Nonsingular Matrix라 하고 그렇지 못한 Matrix를 모두 singular matrix라고 한다.