---
title: "[Linear Algebra]Barycentric Coordinate"
layout: post
date: 2020-01-03 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Graphics
- Linear Algebra
- Barycentric Coordinate
category: blog
author: heetaekim
description: Barycentric Coordinate
MathJax: true
---
# Barycentric Coordinate
---
Affine Space 상에 있는 어떠한 점의 좌표는 Vector Space 하에 점 $O$에 대한 basis vector들로 정의할 수 있다.

$$
\begin{aligned}
    Q = & \vec{u} + \mathcal{O} \\
    =  &a_1\vec{v}_1 + a_2\vec{v}_2 + \dotsi + a_n\vec{v}_n + \mathcal{O}
\end{aligned}
$$

여기서 _basis point_ 라는 대안으로 표현될 수 있는다. 각 basis vector들을 점으로 표현하면

$$
P_0 = O, P_1 = O + \vec{v}_1, P_2 = O + \vec{v}_2, \dotsm, P_n = O + \vec{v}_n
$$

과 같은데 이 것들을 가지고 점 $Q$를 다시 표현하면

$$
Q = P_0a_0 + P_1a_1 + \dotsi + P_na_n
$$

$$
1 = a_0 + a_1 + \dotsi + a_n
$$

이 되는데 이 때, $a_0, a_1, \dotsi, a_n$을 $Q$의 Barycentric Coordinate 라고 한다.

vector도 마찬가지로 표현할 수 있다.

만약 $a_0 = -(a_1 + a_2 + \dotsi + a_n)$이라고 할 때, $\vec{u}$는 아래와 같다.
$$
\vec{u} = a_0P_0 + a_1P_1 + \dotsi + a_nP_n
$$

주의할 점은 계수의 합이 점을 표현할 때와는 다르게 0이라는 것이다. 각 점들(basis point)은 _simplex_ 라고 부른다.(vector는 frame)


### Subspace
vector space가 subspace를 갖는 것처럼 affine space도 갖는데 이 것에 대해 Barycentric Coordinate를 논의할 수 있다. $\mathcal{A}$의 simplex가 $S = (P_0, P_1, \dotsi, P_n)$일 때, 어떠한 subset은 $\mathcal{B} \subset \mathcal{A}$이고 simplex를 $\mathcal{T} = (Q_0, Q_1, \dotsi, Q_m)$으로 갖을 때, 임의의 점 $R \in \mathcal{B}$는 $R = b_0Q_0 + b_1Q_1 + \dotsi + b_mQ_m$ 으로 나타낼 수 있고 계수들의 합은 1이다. 

n-simplex는 n+1 point로 구성되어있으므로 1-simplex는 line이고 2-simplex는 삼각형(평면 정의)이고 3-simplex는 삼각뿔(볼륨 정의)이 된다.

Affine Frame의 basis vector는 linearly independent 여야한다. simplex가 affine space를 정의하는데에 기여된다고 고려했을 때, simplex는 _affinely independent_ 하다고 한다.