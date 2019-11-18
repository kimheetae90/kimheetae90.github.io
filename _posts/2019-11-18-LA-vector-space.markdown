---
title: "[Linear Algebra]Vector Sapce"
layout: post
date: 2019-11-18 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Graphics
- Linear Algebra
- Vector Space
category: blog
author: heetaekim
description: Vector Space
MathJax: true
---
# Vector Space
----
Vector Space는 '방향이 있는 직선'이라고 정의하는 vector 집합으로 구성된 공간으로 아래 법칙을 따른다.

1. vector간의 합이나 차는 다른 vector가 된다.
2. $\mathcal{V}$는 $\vec{u}, \vec{v} \in \mathcal{V}$, $\alpha,\beta \in \mathbb{R}$에 대한 Linear Combination에 대해서 닫혀있다.
3. 아래 2가지를 만족하는 _zero vector_  $\vec{0} \in \mathcal{V}$는 unique하다.
   - $0 \in \mathbb{R}$에 대해, $\forall \vec{V} \in \mathcal{V}, 0 \cdot \vec{v} = \vec{0}$
   - $\forall \vec{V} \in \mathcal{V}, 0 + \vec{v} = \vec{v}$

&nbsp;&nbsp;&nbsp;&nbsp;

# Span
----
$\{ \vec{v_1}, \vec{v_2}, \dots, \vec{v_n} \} \in \mathcal{V}$ 가 주어졌을 때, 이 vector을 Linear Combination한 vector로 이루어진 vector set $\mathcal{S}$는 또 다른 vector space를 만드는데 이 때 이 공간을 _spanned_ 되었다고 한다. 모든 vector $w \in \mathcal{S}$는 

$$ \vec{w} = \lambda_1 \vec{v_1} + \lambda_2 \vec{v_2} + \dots + \lambda_n \vec{v_n}, \lambda_i \in \mathbb{R}$$

로 표현될 수 있다.

3차원에서 표현하자면

![spanned vector space](/assets/images/post/2019-11-18-Vector-Space/span.jpg)

위와 같은 평면에 모든 vector들은 u와 v로 표현할 수 있다.

&nbsp;&nbsp;&nbsp;&nbsp;

# Linear Independence
----
위 예시에서 만약 2개의 vector가 평행했다면 어떨까? 평면을 정의할 수 없을 것이다. 다른 예로 $\vec{u}, \vec{v}, \vec{w}$가 있을 때 만약 $\vec{w} = \alpha\vec{u}$라면 이 세 vector로는 3차원 공간을 정의할 수 없을 것이다. 또 이 때 $\vec{w}$는 $\vec{u}와 \vec{v}$로 _spanned_ 된 공간에 포함되는 vector이다. 이러한 경우를 _linear dependent_ 하다고 한다. 즉 다른 vector들로 한 vector를 표현할 수 있을 때 이며 수식으로 표현하자면

$$ \lambda_1 \vec{v_1} + \lambda_2 \vec{v_2} + \dots + \lambda_n \vec{v_n} = \vec{0}$$

위를 만족하는 0이 아닌 $\lambda$가 존재할 때 이고, 만약 모든 $\lambda$가 0이어야 한다면 이를 _linear independent_ 하다고 한다. _lienar independent_ 하다는 말은 주어진 vector들 사이에서 각각의 vector로 다른 vector를 표현할 수 없다는 말이다.

&nbsp;&nbsp;&nbsp;&nbsp;

# Basis, Subspace, Dimension, Orientation
----
&nbsp;&nbsp;&nbsp;&nbsp;
### Basis
Vector Space $\{ \vec{v_1}, \vec{v_2}, \dots, \vec{v_n}  \} \in \mathcal{S}$가 주어졌을 때
1. $\{ \vec{v_1}, \vec{v_2}, \dots, \vec{v_n}  \}$ 는 linearly independent 하고
2. $\{ \vec{v_1}, \vec{v_2}, \dots, \vec{v_n}  \}$는 $\mathcal{S}$의 spanning set 이라면

$\{ \vec{v_1}, \vec{v_2}, \dots, \vec{v_n}  \}$를 $\mathcal{S}$의 _basis_ 라고 부른다.

&nbsp;&nbsp;&nbsp;&nbsp;
### Subspace
이 때 $\mathcal{V} = \{ \vec{v_1}, \vec{v_2}, \dots, \vec{v_n}  \}$로부터 spanned된 space를 _subspace_ of $\mathcal{S}$라고 부른다.

&nbsp;&nbsp;&nbsp;&nbsp;
### Dimension
그리고 $\mathcal{V}$의 원소의 갯수 $n$을 $\mathcal{S}$의 dimension이라고 한다. $\mathcal{S}$의 dimension은 $n$이다.

예를 들어 위 그림에서 _basis_ 를 $\vec{u}, \vec{v}$라고 했을 때, $\vec{u}, \vec{v}$로 만들 수 있는 공간은 _subspace_ 가 되고 평면 위에 있을 것이다. 그리고 이 평면의 _dimension_ 은 $n$이 된다.

여기서 중요한 것이 있는데 

$$\vec{w} = x_1\vec{v_1} + x_2\vec{v_2} + \dots + x_n\vec{v_n}, x_i \in \mathbb{R}$$

인 $\vec{w}$가 있을 때, $\vec{w}$를 만들 수 있는 $x_i$는 unique 해야한다. 이 때 $x_i\vec{v_i}$를 $\vec{w}$의 _components_ 라고 부르고 $x_i$를 $\vec{w}$의 _corrdinates_ 라고 부른다.


&nbsp;&nbsp;&nbsp;&nbsp;
### Orientation
두 vector $\vec{u}, \vec{v}$가 있을 때 두 vector 사이의 각을 $\theta_{\vec{u}\vec{v}}$라고 표현한다. vector는 사전식으로 읽고 사이각은 order$(\theta_{\vec{u}\vec{v}})$ 라고 표기한다. 사이각의 화살표는 아래와 같이 $\vec{u}$에서 $\vec{v}$로 가도록 그리고 이를 양수로 간주한다. 이 것을 "counterclockwise"라고 부른다.

![angle](/assets/images/post/2019-11-18-Vector-Space/angle.jpg)

이 때 좌표계를 정의할 수 있는데 위 그림에서 vector 하나를 더 추가해보자. 두 vector가 이루는 spcae는 모니터와 평행하다고 할 때 모니터 안에서 밖으로 나오는 vector $\vec{w}$가 있다고 가정하자. 이 때 세 vector가 이루는 방향의 관계를
$$sgn(\vec{u}, \vec{v}, \vec{w}) = sgn(\theta_{\vec{u}\vec{v}})$$
로 표기한다.
$sgn(\vec{u}, \vec{v}, \vec{w})$를 만든 후 오른손 엄지손가락으로 $\vec{w}$ 방향을 가르켰을 때 나머지 손가락이 $\vec{u}, \vec{v}$를 순서로 따라가는 것, 즉 위에서 말한 "counterclockwise"를 따라간다면 이를 오른손 좌표계라고 하고 $sgn(\vec{u}, \vec{v}, \vec{w}) = +1$로 표기한다.

&nbsp;&nbsp;&nbsp;&nbsp;

# Change of Basis
----
$\mathcal{V}$를 이루는 두 basis vector $\vec{a_1}, \vec{a_2}, \dots, \vec{a_n}$과 $\vec{b_1}, \vec{b_2}, \dots, \vec{b_n}$가 있다고 하자. 이 때 $\vec{w} \in \mathcal{V}$을 표현할 때, $\vec{a_1}, \vec{a_2}, \dots, \vec{a_n}$들의 Linear Combination으로 표현할 수 있다.
$$\vec{w} = c_1\vec{a_1} + c_2\vec{a_2} + \dots + c_n\vec{a_n}$$
또 $\vec{a_i}$ 자체도 $\vec{b_1}, \vec{b_2}, \dots, \vec{b_n}$들의 Linear Combination으로 표현할 수 있기 때문에
$$\vec{a_i} = d_{i,1}\vec{b_1} + d_{i,2}\vec{b_2} + \dots + d_{i,n}\vec{b_n} $$
로 표현할 수 있다.
이를 대입해보면

$$\vec{w} = c_1\vec{a_1} + c_2\vec{a_2} + \dots + c_n\vec{a_n}\\
 = c_1(d_{1,1}\vec{b_1} + d_{1,2}\vec{b_2} + \dots + d_{1,n}\vec{b_n}) \\
 + c_2(d_{2,1}\vec{b_1} + d_{2,2}\vec{b_2} + \dots + d_{2,n}\vec{b_n}) \\
 + \dots +  c_n(d_{n,1}\vec{b_1} + d_{n,2}\vec{b_2} + \dots + d_{n,n}\vec{b_n}) \\
 = (d_1c_{1,1} + d_2c_{1,2} + \dots + d_nc_{1,n})\vec{b_1} \\
 + (d_1c_{2,1} + d_2c_{2,2} + \dots + d_nc_{2,n})\vec{b_2} \\
 + \dots + (d_1c_{n,1} + d_2c_{n,2} + \dots + d_nc_{n,n})\vec{b_n}
$$

과 같이 된다. 이는 행렬 곱셈을 통해 간단히 수행될 수 있다.


&nbsp;&nbsp;&nbsp;&nbsp;

# Linear Transformations
----
_linear transformation_ 은 한 vector space에서 다른 vector space로 maaping되는 것을 일컷는다. 즉 $\mathcal{U}, \mathcal{V}, T : \mathcal{U} \rightarrow \mathcal{V}$가 있을 때

1. $T(\vec{u} + \vec{v}) = T(\vec{u}) + T(\vec{v})$, $\vec{u}, \vec{v} \in \mathcal{V}$
2. $T(\alpha\vec{v}) = \alpha T(\vec{u})$, $\alpha \in \mathbb{R} \vec{u} \in \mathcal{V}$

을 만족한다.

즉 linear transformation은 linear combination을 유지하기 때문에 linear transformation을 통해 길이변경, 방향전환 등을 표현할 수 있다.