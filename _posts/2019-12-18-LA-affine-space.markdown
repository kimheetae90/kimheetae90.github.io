---
title: "[Linear Algebra]Affine Sapce"
layout: post
date: 2019-12-18 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Graphics
- Linear Algebra
- Affine Space
category: blog
author: heetaekim
description: Affine Space
MathJax: true
---
# Vector와 Point의 표기
---
vector는 소문자로 표기되고 문자 위에 화살표가 있거나 hat이 있다.


$\vec{a}$ 혹은 $\hat{a}$



점은 대문자로 표기한다

$P$ 혹은 $Q$

이 때 $P$에서 $Q$로 가는 vector는 $\vec{pq}$ 로도 표현한다.

# Affine Space
---
우선 직관적으로는 Affine Space $\mathcal{A}$는 Point Set $\mathcal{P}$와 Vector Set $\mathcal{V}$로 이루어져 있다. 이 때 $\mathcal{A}$의 _dimension_ 은 $\mathcal{V}$의 _dimension_ 과 같다.

Affine Space $\mathcal{A}$안에 있는 Point들을 $\mathcal{A}.\mathcal{P}$ 라고 표현하고 Vector들을 $\mathcal{A}.\mathcal{V}$ 라고 표현한다.

좀 더 Formal하게 $\mathcal{A}$에서의 $\mathcal{P}$와 $\mathcal{V}$의 관계를 표현하면 Point 쌍의 차에 의해 정의되는 _Head To Tail_ 이라는 공리법칙을 따르는 공간이다.

1. $\forall P, Q \in \mathcal{A}.\mathcal{P}$ 일 때, $\vec{v} = P - Q$ 인 고유한 vector $\vec{v} \in \mathcal{A}$. $\mathcal{V}$ 가 존재한다. 
2. $\forall Q \in \mathcal{A}.\mathcal{P}, \forall\vec{v} \in \mathcal{A}.\mathcal{V}$ 일 때, $P - Q = \vec{v}$ 인 $P$ 가 존재한다.
3. $\forall P, Q, R \in \mathcal{A}.\mathcal{P}$ 일 때, $(P - Q) + (Q - R) = P - R$
4. $\forall P \in \mathcal{A}.\mathcal{P}, 1 \cdot P = P$, 그리고 $0 \cdot P = \vec{0}$

그리고 Affine Space는 아래 특징을 갖고 있다.

1. $Q - Q = \vec{0}$
2. $R - Q = -(Q - R)$
3. $\vec{v} + (Q - R) = (Q + \vec{v}) - R$
4. $Q - (R + \vec{v}) = (Q - R) - \vec{v}$
5. $P = Q + (P - Q)$
6. $(Q + \vec{v}) - (R + \vec{w}) = (Q - R) + (\vec{v} - \vec{w})$

&nbsp;&nbsp;&nbsp;&nbsp;

# Affine Combination
---
Affine Space 상에서는 Point에 Scalar를 곱하는 행위, Point들끼리의 합이 금지되어있다. 여기서 두 번째인 Point끼리의 합을 살펴보자.
예를 들어, $x + y + 1 = z$ 인 평면이 있을 때 이 안에서의 Point끼리의 합은 금지이다. 왜냐하면 $(x_{1} + x_{2} + y_{1} + y_{2} + 2 = z_{1} + z_{2})$가 되어 평면에서 벗어나버리기 떄문이다. 

하지만 Point끼리의 합이 허용될 때가 있다. $P_{1}$과 $P_{2}$의 상수배의 합을 표현할 때 $\alpha P_{1} + \beta P_{2}$ 라고 표현하자. 이 때 $\alpha$와 $\beta$의 합이 1이라면 가능하다. 즉 $\alpha + \beta = 1$ 이므로 $\beta = 1 - \alpha$ 가 된다. 그럼 새로운 점은 
$$R = \alpha_{1}P + \alpha_{2}Q \\
 (\alpha_{1} + \alpha_{2} = 1)$$
가 된다.

이를 일반화하면 _affine combination_ 은 아래와 같이 정의할 수 있다.

$$\alpha_{1}P_{1} + \alpha_{2}P_{2} + \dots + \alpha_{n}P_{n}$$

&nbsp;&nbsp;&nbsp;&nbsp;

# Euclidean Geometry
---
Affine Space에서는 origin에 대해서 언급하지 않았다. 그리고 각도와 길이에 대해서 명확하게 언급하지 않았다. 유클리드는 자신이 생각하는 길이와 각도를 좌표계에 도입해 _Euclidean Space_ 를 만들었다. _Euclidean Space_ 는 _Affine Space_ 의 subset이 된다. Affine Space 상에서 적용되었던 법칙들은 Euclidean Space에서도 적용이 된다. 

우리는 앞서 vector를 더하고 빼는 행위와 vector에 scalar를 곱하는 행위에 대해 보았다. 이 외에 2가지 연산이 더 존재하는데 이 연산들은 두 vector를 곱해서 나오는 연산이다. single-valued를 만드는 _scalar product_ 와 또 다른 vector를 만드는 _vector product_ 이다.

## Scalar Product
_dot product_ 라고 알려져 있고 _inner product_ 라고도 부른다. 특성을 알아보기 전에 몇가지 알아야할 정의가 있다.

* Length : $\vec{u}$의 길이이고 $\|\|\vec{u}\|\|$ 라고 쓴다.
* Direction : $\vec{u}$의 방향이고 $dir(\vec{u})$ 라고 쓴다.
* Sign : $\alpha$의 부호이고 $sgn(\alpha)$ 라고 쓴다.
* Perpendicular : $\vec{u}와 \vec{v}$가 서로 수직이면 $\vec{u}\perp\vec{v}$ 라고 쓴다.
* Paraller : $\vec{u}와 \vec{v}$가 서로 수직이면 $\vec{u}\parallel\vec{v}$ 라고 쓴다.

또, _proejction_ 은 아래와 같다.

![Vector Projection](/assets/images/post/2019-12-08-Affine-Space/projection.jpg)

위 그림에서 우리는 두 가지 요소를 볼 수 있다.

* $\vec{v}_{\perp}$ : $\vec{u}$ 에 수직인 $\vec{v}$의 요소로 normal component 라고 부른다
* $\vec{v}_{\parallel}$ : $\vec{u}$와 평행인 $\vec{v}$의 요소로 orthogonal projection 이라고 부른다.

여기서 볼 수 있는 성질은 아래와 같다.

* $\vec{v}_ {\perp} + \vec{v}_{\parallel} = \vec{v}$
* $\|\|\vec{v}_{\perp}\|\| = \|\|\vec{v}\|\|\|\sin\theta\|$
* $\|\|\vec{v_{\parallel}}\|\| = \|\|\vec{v}\|\|\|\cos\theta\|$
* $\vec{v}_{\parallel} = \|\|\vec{v}\|\|\cos\theta\hat{u}$, $\hat{u}$는 unit vector이고 값은 $\frac{\vec{u}}{\|\|\vec{u}\|\|}$

&nbsp;&nbsp;&nbsp;&nbsp;

#### Scalar Product의 특성
_dot product_ 는 몇가지 특성을 가지고 있다. 그 특성은 아래와 같다.

1. 정의: $\vec{u}\cdot\vec{v} = \|\|\vec{u}\|\|\|\|\vec{v}\|\|cos\theta$
2. 쌍선형: $\forall\alpha\beta \in \mathbb{R}$, $\forall\vec{u},\vec{v},\vec{w} \in \mathcal{A}.\mathcal{V}$,
$(\alpha\vec{u} + \beta\vec{v})\cdot\vec{w} = \alpha(\vec{u}\cdot\vec{w}) + \beta(\vec{v}\cdot\vec{w})$
$\vec{u}\cdot(\alpha\vec{v} + \beta\vec{w}) = \alpha(\vec{u}\cdot\vec{v}) + \beta(\vec{u}\cdot\vec{w})$
3. 정부호 : $\forall\vec{u} \in \mathcal{A.V}, \vec{u} \neq \vec{0}, \vec{u}\cdot\vec{u} \gt 0$
$\vec{0}\cdot\vec{0} = 0$
4. 교환법칙: $\vec{u}\cdot\vec{v} = \vec{v}\cdot\vec{u}$
5. vector간의 합에 대한 _dot product_ 의 분배법칙: $\vec{u}\cdot(\vec{v} + \vec{w}) = (\vec{u}\cdot\vec{v}) + (\vec{u}\cdot\vec{w})$
6. _dot product_ 에 대한 vector 간의 합의 분배법칙: $(\vec{u} + \vec{v})\cdot\vec{w} = \vec{u}\cdot\vec{w} + \vec{v}\cdot\vec{w}$

위 내용들을 바탕으로 아래와 같이 정리할 수 있다.

1. Squared length : $\vec{u}\cdot\vec{u} = \|\|\vec{u}\|\|^{2}$
2. Angle : $\theta = cos^{-1}\frac{\vec{u}\cdot\vec{v}}{\|\|\vec{u}\|\|\|\|\vec{v}\|\|}$
3. Projection : $\vec{v}_{\parallel} = \frac{(\vec{u}\cdot\vec{v})\vec{u}}{\vec{u}\cdot\vec{u}}$
4. Normal : $\vec{v}_\perp = \vec{v} - \frac{(\vec{u}\cdot\vec{v})\vec{u}}{\vec{u}\cdot\vec{u}}$
5. Perpendicular : $\vec{u}\cdot\vec{v} = 0 \Longleftrightarrow \vec{u} \perp \vec{v}$


&nbsp;&nbsp;&nbsp;&nbsp;


## Vector Product

_cross product_ 라고 알려져 있고 _outer product_ 라고도 부른다. _cross product_ 는 두 vector가 만드는 평면의 방향을 정의할 수 있다. vector $\vec{u}, \vec{v}$가 있을 때 _cross product_ 를 하면 새로운 vector $\vec{w}$가 나오는데 이 것은 두 vector와 동시에 수직인 vector이다. _cross product_는  $\times$ 로 표현한다.

&nbsp;&nbsp;&nbsp;&nbsp;

### Vector Product의 특성
1. 정의 : $\|\|\vec{u}\times\vec{v}\|\| = \|\|\vec{u}\|\| \|\|\vec{v}\|\|sin\theta$
2. Anticommutativity : $\vec{u}\times\vec{v} \neq \vec{v}\times\vec{u}$
3. Distributivity : $\vec{u}\times(\vec{v} + \vec{w}) = (\vec{u}\times\vec{v}) + (\vec{u}\times\vec{v})$ , $(\alpha\vec{u})\times\vec{v} = \vec{u}\times(\alpha\vec{v}) = \alpha(\vec{u}\times\vec{v})$
4. Parallelism: $\vec{u}\|\|\vec{v} = \Longleftrightarrow \vec{u}\times\vec{v} = \vec{0}$

추가로 오른손 법칙을 따라서 두 vector $\vec{u}, \vec{v}$의 사이각  $\theta > 0$이면, _cross product_ 로 생성되는 vector는 화면에서 나오는 방향으로 향할 것이고 $\theta < 0$이면 그 반대 방향이다.

또, 두 vector가 이루는 평행사변형의 넓이를 나타내기도 하는데 주의할 점은 $\theta > 0$이면  $sin\theta$의 절대 값을 사용해야 한다

&nbsp;&nbsp;&nbsp;&nbsp;
