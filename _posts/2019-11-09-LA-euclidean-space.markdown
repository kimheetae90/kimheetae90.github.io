---
title: "[Linear Algebra]Euclidean Space"
layout: post
date: 2019-11-09 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Graphics
- Linear Algebra
- Euclidean Space
category: blog
author: heetaekim
description: Euclidean Space
MathJax: true
---
# Euclidean Space
----
Computer Graphics에서는 Linear Space의 subclass인 Euclidean Space가 중요한 부분을 차지한다.

&nbsp;&nbsp;&nbsp;&nbsp;

# Inner Product Space
----
$\mathcal{V}$는 $mathds{R}^n$에 대한 vector space라고 할 때 $\mathcal{V}^2$를 $\vec{u}, \vec{v} \in \mathcal{V}$인 모든 $(\vec{u}, \vec{v})$ 쌍의 집합이라고 하겠다. 이 때 _inner product_ 는 $\mathcal{V}^2$에서 $mathds{R}$으로 변환하는 function이고 $\langle \vec{u}, \vec{v} \rangle$ 이라고 표기한다. _inner product_ 는 아래 세가지 규칙을 만족한다.

1. 분배법칙 : $\langle a_1\vec{u_1} + a_2\vec{u_2}, \vec{v}\rangle = a_1\langle \vec{u_1}, \vec{v_1} \rangle + a_2\langle \vec{u_2} , \vec{v} \rangle$
2. 교환법칙 : $\langle \vec{u}, vec{v} \rangle = \langle \vec{v} , \vec{u} \rangle$
3. 양의 정부호 : $\langle \vec{u}, \vec{v} \rangle \geq 0$ 이고 $\langle \vec{u}, \vec{v} \rangle = 0 \Longleftrightarrow \vec{u} = \vec{0}

&nbsp;&nbsp;&nbsp;&nbsp;

### Norm, Length, Distance
_Norm_ 이란 Vector Space상에서의 vector의 크기를 지칭하는 말이다. _Norm_ 에는 몇가지 종류가 있다. L1, L2, Max Norm이 있는데 Eclidean Space에서는 L2 Norm을 사용한다. Eclidean Space에서의 _Norm_ 을 vector의 _length_ 라고 보는 것을 허용한다. 공식은 아래와 같다.

$$\|\vec{u}\| = \sqrt{\langle \vec{u}, \vec{u}}$$

여기서 만약 vector의 norm, 즉 길이가 1이라면 ($\|\vec{u}\| = 1$) 이 vector를 _normalized_ 되었다고 한다. _nomalize_ 하는 방법은 norm에 역을 취하는 것이다. 즉 $\frac {1}{\|\vec{u}\|}$ 이다.

또, $\vec{v}, \vec{u} \in \mathcal{V}$ 일 때 두 vector간의 거리를 _distance_ 라고 한다. 이는 \|\vec{u} - \vec{v}\|로 정의한다.

&nbsp;&nbsp;&nbsp;&nbsp;

# Orthogonality
----
Euclidean Space에서 _inner produce_ 가 0인 것, 즉 $\langle \vec{u}, \vec{v} \rangle = 0$ 인 두 vector를 _othogonal_ 하다고 부른다.

아 카페에서 나가래 ㅡㅡ

