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
# Linear Space
----
이 포스팅에서는 Linear Space, 혹은 Vector Space 라는 것에 대해 알아볼 것이다. Linear Sapce는 여러 vector들이 모여서 이룬 공간을 말한다 이 공간에서는  원소를 서로 더하거나 주어진 배수로 늘일 수 있다.

&nbsp;&nbsp;&nbsp;&nbsp;

# Fields
----
자세히 알아보기에 앞서 Fields(체)라는 것을 먼저 알아보겠다. Fields는 시스템 내에서 요소들간 사칙연산이 가능한 대수학적 시스템이다. 그리고 이 시스템에서는 연관, 교환, 분배 법칙이 적용된다. Fields는 commutative ring, commutative division algebra 라고도 불린다. Fields는 아래 규칙들을 만족한다.

1. 덧셈과 곱셈에 대해 닫혀있다. $\forall a,b \in F$, both $(a + b) \in F$ and $(a * b) \in F$
2. 덧셈과 곱셈의 연관법칙이 성립한다. $\forall a,b,c \in F$, both $(a + b) + c = a + (b + c)$ and $(a * b) * c = a * (b * c)$
3. 덧셈과 곱셈의 교환법칙이 성립한다. $\forall a,b \in F$, both $a + b = b + a$ and $a * b = b * a$
4. 덧셈과 곱셈의 분배법칙이 성립한다. $\forall a,b,c \in F$, both $a * (b + c) = (a * b) + (a * c)$ and $(b + c) * a = (b * a) + (c * a)$
5. 덧셈의 항등원이 존재한다. $\exists 0 \in F$ such that $\forall a \in F, a + 0 = a$ and $0 + a = a$
6. 곱셈의 항등원이 존재한다. $\exists 1 \in F$ such that $\forall a \in F, a * 1 = a$ and $1 * a = a$
7. 덧셈의 역원이 존재한다. $\forall a \in F, \exists -a \in F$ such that $a + (-a) = 0$ and $(-a) + a = 0$
8. 곱셈의 역원이 존재한다. $\forall a \neq 0 \in F, \exists a^{-1} \in F$ such that $a * a^{-1} = 1$ and $a^{-1} * a = 1$

Fields의 예로는 유리수, 실수, 복소수 등이 있다. 정수는 곱셈의 역원이 존재하지 않을 수도 있기 때문에 Field가 아니다.


&nbsp;&nbsp;&nbsp;&nbsp;

# Linear Space의 특성
----
비공식적으로 Linear Space는 vector, 실수와 덧셈,곱셈의 집합이다. 공식적으로는 아래 법칙들을 만족한다.

1. A Field $K$
2. A Vector Set $\mathcal{V}$
3. An $\vec{u},\vec{v} \in \mathcal{V}$에 대한 덧셈 연산자 $+$
4. A Scalar $k \in K$와 $\vec{v} \in \mathcal{V}$에 대한 곱셈 연산자 $*$

라고 했을 때 덧셈, 곱셈 연산자에 대해 아래 나열된 규칙을 만족한다.

1. 곱셈에 대해 닫혀있다. $\forall k \in K$ and $\forall \vec{v} \in \mathcal{V}$, $k \vec{v} \in \mathcal{V}$
2. 덧셈에 대해 닫혀있다. $\forall \vec{u},\vec{v} \in \mathcal{V}$, $\vec{u} + \vec{v} \in \mathcal{V}$
3. 덧셈에 대한 결합법칙이 성립한다. $\forall \vec{u}, \vec{v}, \vec{w} \in \mathcal{V}$, $\vec{u} + (\vec{v} + \vec{w}) = (\vec{u} + \vec{v}) + \vec{w}$
4. 덧셈의 항등원이 존재한다. $\forall \vec{v} \in \mathcal{V}$, $\exists \vec{0} \in \mathcal{V}$ (zero vector), such that $\vec{v} + \vec{0} = \vec{v}$
5. 덧셈의 역원이 존재한다. $\forall \vec{v} \in \mathcal{V}$, $\exists -\vec{v}$, such that $\vec{v} + (-\vec{v}) = \vec{0}$
6. 덧셈의 교환법칙이 성립한다. $\forall \vec{u},\vec{v} \in \mathcal{V}$, $\vec{u} + \vec{v} = \vec{v} + \vec{u}$
7. 덧셈과 곱셈의 분배법칙이 성립한다. $\forall k \in K$ and $\forall \vec{u},\vec{v} \in \mathcal{V}$, $k(\vec{u} + \vec{v}) = k\vec{u} + k\vec{v}$
8. 곱셈과 덧셈의 분배법칙이 성립한다. $\forall k_{1}, k_{2} \in K$ and $\forall \vec{v} \in \mathcal{V}$, $(k_{1} + k_{2})\vec{v} = k_{1}\vec{v} + k_{2}\vec{v}$
9. 곱셈의 교환법칙이 성립한다. $\forall k_{1},k_{2} \in K$, and $\forall \vec{v} \in \mathcal{V}$, $(k_{1}k_{2})\vec{v} = k_{1}(k_{2}\vec{v})$
10. 곱셈의 항등원이 존재한다. $\forall \vec{v} \in \mathcal{V}$, $1 * \vec{v} = \vec{v}$


&nbsp;&nbsp;&nbsp;&nbsp;

# Subspaces
----
Linear Space V가 있을 때 S를 V의 subset이라고 하고 S와 V의 연산(덧셈, 곱셈)이 같다고 하면 S를 V의 _Subspace_라고 한다.


&nbsp;&nbsp;&nbsp;&nbsp;

# Linear Combination
----
Linear Combination은 vector들의 scalar배와 vector 덧셈들을 통해 조합해 새로운 vector를 얻는 연산이다. Linear Space에서 가장 기본적인 연산이다. vector set $\mathbb{R}^{n} : $  $\{ \vec{a_{1}}, \vec{a_{2}},\dots, \vec{a_{n}} \}$ 에서 $\mathcal{A}$: $\{ \vec{a_{1}}, \vec{a_{2}},\dots, \vec{a_{n}} \}$에서 vector $\vec{u} = k_{1}\vec{a_{1}} + k_{2}\vec{a_{2}} + \dots + k_{n}\vec{a_{n}}$ 와 같이 표현한다.

### Span
$\mathbb{V} : $  $\{ \vec{v_{1}}, \vec{v_{2}},\dots, \vec{v_{n}} \}$ 에서 $\mathbb{V}$ 안에 존재하는 vector들의 Linear Combination으로 확장된 공간을 _spanned_ 되었다고 하고 이 공간은 다른 Linear Space가 된다. 이 set을 _spanning set_ 이라고 한다.


&nbsp;&nbsp;&nbsp;&nbsp;

# Linear Independence
----
$\mathbb{V} : \{ \vec{v_{1}}, \vec{v_{2}},\dots, \vec{v_{n}} \}$가 주어졌을 때, 

$$c_{1}\vec{v_{1}} + c_{2}\vec{v_{2}} + \dots + c_{n}\vec{v_{n}} = \vec{0}$$

을 만족하는 해가 모든 constants가 0인 것 밖에 없을 때 _Linearly independent_ 하다고 한다. 그렇지 않은 경우를 _linearly dependent_라고 한다.



&nbsp;&nbsp;&nbsp;&nbsp;

# Base and Dimension
----
Vector set $\{ \vec{v_{1}}, \vec{v_{2}},\dots, \vec{v_{n}} \}$ 가 주어졌을 때, 모든 요소(vector)들이 서로 linearly independent한 space $\mathbb{V}$를 span했을 때 $\mathbb{V}$ 의 요소들을 _basis_라고 하고 $\mathbb{V}$의 vector 갯수를 _dimension_ 이라고 한다. 여기서 몇가지 정의를 알 수 있다.

1. $n$개보다 적은 lineary dependent한 vector들은 $\mathbb{V}$ 는 $\mathbb{V}$ 에서 span될 수 없다.
2. $\mathbb{V}$ 내에 $n$보다 많은 수의 vector들로 된 vector set은 무조건 linearly dependent 하다.
3.  dimension이 $n$ 인 $\mathbb{V}$ 의 basis는 무수히 많다.