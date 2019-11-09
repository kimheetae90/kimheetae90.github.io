---
title: "[Linear Algebra]Eigen Value & Eigen Vector"
layout: post
date: 2019-11-09 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Graphics
- Linear Algebra
- Eigen Value
- Eigen Vector
category: blog
author: heetaekim
description: Eigen Value, Eigen Vector
MathJax: true
---
# Eigen Vector와 Eigen value
----
Geometric적인 관점에서 vector는 "Space"에서 방향과 크기를 갖는 것으로 볼 수 있다. 이 때 vector에 matrix를 곱하는 행위는 해당하는 vector의 속성, 즉 방향이나 크기를 transform하는 행위라고 볼 수 있다. vector $\vec{v}$에 matrix $M$을 곱하는 것은 다음과 같이 표기할 수 있다.

$$ \vec{v^{'}} = \vec{v} M $$

여기서 어떤 특수한 vector $\vec{v}$ 는 다음과 같은 식을 만족할 때가 있다.

$$ \vec{v^{'}} = \lambda \vec{v} $$

이를 만족하는 vector는 다음과 같은 식이 유추될 수 있다.

$$ \vec{v} M = \lambda \vec{v} $$

다음과 같은 $\lambda$를 구할 수 있는 vector $\vec{v}$를 matrix $M$의 _eigen vector_ 라고 부르고 그 때의 scalar 값인 $\lambda\를 _eigen value_ 라고 한다. 즉, matrix $M$에 의해 Linear Transform된 결과가 자기 자신의 상수배가 되는 0이 아닌 vector를 _eigen vector_ 라고 하고 그 때의 상수배 값을 _eigen value_ 라고 하는 것이다.

&nbsp;&nbsp;&nbsp;&nbsp;

# Eigen Vector 구하기
----
위에서 도출한 마지막 공식을 변환하면 아래와 같아진다.

$$ \vec{v}(\lambda I - M) = \vec{0} $$

이 식에서 $\vec{v}$가 0이 아니라면 $|\lambda I - M|$ = 0$ 을 만족해야한다. 
이 를 _characteristic polynomial_ 라고 부른다. 예시를 들어보자!

$$
M = 
\begin{bmatrix} 
6 & 4 \\ 
2 & 4 
\end{bmatrix}
$$

에 대해 _characteristic polynomial_ 을 만족하기 위해서는

$$
\begin{vmatrix} 
\lambda - 6 & -2 \\ 
-4 & \lambda - 4 
\end{vmatrix}
 = 
\lambda^2 - 10\lambda + 16 = (\lambda - 8)(\lambda - 2) = 0
$$

가 성립되어야 하므로 $\lambda$는 8, 2 이다. 이를 $\vec{v} M = \lambda \vec{v}$에 대입한다면

&nbsp;&nbsp;&nbsp;&nbsp;

$\lambda$가 2일 때에는 

$$
2\vec{v_1} - 2\vec{v_2} = 0 \\
-4\vec{v_1} + 4\vec{v_2} = 0
$$

$\vec{v} = \begin{bmatrix} 1 & 1 \end{bmatrix}$ 를 얻을 수 있고

&nbsp;&nbsp;&nbsp;&nbsp;

$\lambda$가 8일 때에는 

$$
-4\vec{v_1} - 2\vec{v_2} = 0 \\
-4\vec{v_1} - 2\vec{v_2} = 0
$$

$\vec{v} = \begin{bmatrix} 1 & -2 \end{bmatrix}$ 를 얻을 수 있다.

# General Form의 해
----
eigen vector와 eigen value가 존재하지 않을 수도 있고 한개만 존재하거나 $n$개가 존재할 수도 있다. 