---
title: "[Linear Algebra]Products of Vectors"
layout: post
date: 2020-01-03 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Graphics
- Linear Algebra
category: blog
author: heetaekim
description: Products of Vectors
MathJax: true
---
# Products of Vectors
---
### Dot Product
수직인 두 vector의 cosin은 0이고 평행한 두 vector의 cosin은 1이기 때문에 basis간의 dot product는 다음과 같다.

$$
\vec{i}\cdot\vec{i} = ||\vec{i}||||\vec{i}||cos\theta = 1 \cdot 1 \cdot 1 = 1 \\
\vec{j}\cdot\vec{j} = ||\vec{j}||||\vec{j}||cos\theta = 1 \cdot 1 \cdot 1 = 1 \\
\vec{k}\cdot\vec{k} = ||\vec{k}||||\vec{k}||cos\theta = 1 \cdot 1 \cdot 1 = 1
$$


$$
\vec{i}\cdot\vec{j} = ||\vec{i}||||\vec{j}||cos\theta = 1 \cdot 1 \cdot 0 = 0 \\
\vec{j}\cdot\vec{k} = ||\vec{j}||||\vec{k}||cos\theta = 1 \cdot 1 \cdot 0 = 0 \\
\vec{k}\cdot\vec{i} = ||\vec{k}||||\vec{i}||cos\theta = 1 \cdot 1 \cdot 0 = 0
$$

이 때, $\vec{u}$와 $\vec{v}$가 다음과 같다고 하자.

$$
\vec{u} = u_1 \vec{i} + u_2 \vec{j} + u_3 \vec{k} \\
\vec{v} = v_1 \vec{i} + v_2 \vec{j} + v_3 \vec{k}
$$

이 때, Dot product는 다음과 같이 계산할 수 있다.

$$
\begin{aligned}
    \vec{u} \cdot\vec{v} = & [u_1, u_2, u_3, 0] \cdot [v_1, v_2, v_3, 0] \\
    = & (u_1 \vec{i} + u_2 \vec{j} + u_3 \vec{k} ) \cdot (v_1 \vec{i} + v_2 \vec{j} + v_3 \vec{k}) \\
    = & u_1v_1(\vec{i}\cdot\vec{i}) + u_1v_2(\vec{i}\cdot\vec{j}) + u_1v_3(\vec{i}\cdot\vec{k}) \\
    + & u_2v_1(\vec{j}\cdot\vec{i}) + u_2v_2(\vec{j}\cdot\vec{j}) + u_2v_3(\vec{j}\cdot\vec{k}) \\
    + & u_3v_1(\vec{k}\cdot\vec{i}) + u_3v_2(\vec{k}\cdot\vec{j}) + u_3v_3(\vec{k}\cdot\vec{k}) \\
    = & u_1v_1 + u_2v_2 + u_3v_3
\end{aligned}
$$

&nbsp;&nbsp;&nbsp;&nbsp;
### Cross Product


&nbsp;&nbsp;&nbsp;&nbsp;
### Tensor Product


