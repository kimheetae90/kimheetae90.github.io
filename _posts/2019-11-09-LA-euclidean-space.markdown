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
3. 양의 정부호 : $\langle \vec{u}, \vec{v} \rangle \geq 0$ 이고 $\langle \vec{u}, \vec{v} \rangle = 0 \Longleftrightarrow \vec{u} = \vec{0}$

&nbsp;&nbsp;&nbsp;&nbsp;

### Norm, Length, Distance
_Norm_ 이란 Vector Space상에서의 vector의 크기를 지칭하는 말이다. _Norm_ 에는 몇가지 종류가 있다. L1, L2, Max Norm이 있는데 Eclidean Space에서는 L2 Norm을 사용한다. Eclidean Space에서의 _Norm_ 을 vector의 _length_ 라고 보는 것을 허용한다. 공식은 아래와 같다.

$$\|\vec{u}\| = \sqrt{\langle \vec{u}, \vec{u} \rangle}$$

여기서 만약 vector의 norm, 즉 길이가 1이라면 ($\|\vec{u}\| = 1$) 이 vector를 _normalized_ 되었다고 한다. _nomalize_ 하는 방법은 norm에 역을 취하는 것이다. 즉 $\frac {1}{\|\vec{u}\|}$ 이다.

또, $\vec{v}, \vec{u} \in \mathcal{V}$ 일 때 두 vector간의 거리를 _distance_ 라고 한다. 이는 $\|\vec{u} - \vec{v}\|$로 정의한다.

&nbsp;&nbsp;&nbsp;&nbsp;

# Orthogonality
----
Euclidean Space에서 _inner produce_ 가 0인 것, 즉 $\langle \vec{u}, \vec{v} \rangle = 0$ 인 두 vector를 _orthogonal_ 하다고 부른다. Orthogonality는 basis vector에서 중요한 역할을 한다. $\mathcal{V} = {\vec{v_1}, \vec{v_2}, \dots, \vec{v_n}}$을 vector space $\vec{v}$에서의 basis라고 하자. 이 때 $\langle \vec{v_i}, \vec{v_k} \rangle = 0, \forall \vec{v_i}, \vec{v_k} \in \mathcal{V}, i \neq k$이면 $\mathcal{V}$를 _orthogonal set_ 이라고 부른다. 그리고 set 안에 모든 vector가 normal vector라면 _orthonormal_ 이라고 부른다. 또, _orthonormal_ 로 이루어진 Eclidean Space를 _Cartesian_ space라고 한다.

&nbsp;&nbsp;&nbsp;&nbsp;

### Gram-Schmidt Orthogonalization
그람 슈밋 직교화란 주어진 vector들로부터 이 vector들을 생성할 수 있는 orthogonal basis vector를 구하는 과정이다. 과정은 비교적 간단하다.

$\mathcal{V} = {\vec{v_1}, \vec{v_2}, \dots, \vec{v_n}}$ 가 있을 때

1. $\vec{u_1} = \vec{v_1}$ 이다.
2. $\vec{u_2} = \vec{v_2} - Proj_{u_1}(v_2)$ 이다. 즉, $\vec{u_2} = \vec{v_2} - \langle \vec{v_2}, \vec{u_1} \rangle \vec{u_1}$
3. $\vec{u_3} = \vec{v_2} - Proj_{u_1}(v_3) - Proj_{u_2}(v_3)$ 이다. 즉, $\vec{u_2} = \vec{v_3} - \langle \vec{v_3}, \vec{u_1} \rangle \vec{u_1} - \langle \vec{v_3}, \vec{u_2} \rangle \vec{u_2}$
4. 이를 반복한다. 일반화하면 $vec{u_k} = vec{v_k} - \sum_{i=1}^{k-1}{Proj_{u_i}(v_k)}$이다.

이렇게 생성된 $\vec{u_k}$들을 _normalize_ 하면 _Gram-Schmidt orthonormalization_ 가 되고 이는 $\mathcal{V}$의 basis가 된다.