---
title: "[Linear Algebra]Linear Mapping"
layout: post
date: 2019-11-08 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Graphics
- Linear Algebra
- Linear Mapping
category: blog
author: heetaekim
description: Linear Mapping
MathJax: true
---

# Linear Mapping
----
Linear Mapping은 두 Vector Space $\mathcal{A}$와 $\mathcal{B}$가 있을 때 $\mathcal{A}$에서 $\mathcal{B}$로 변환하는 함수를 말한다. Linear Transformation, Linear Operation 이라고도 한다. 

&nbsp;&nbsp;&nbsp;&nbsp;

# 일반적인 Mapping
----
가장 기본적인 아이디어는 어떠한 집합에 있는 멤버를 다른 집합의 멤버와 연관시키는 규칙이다.


**정의**
- $\mathcal{A} = \{ a_{1}, a_{2}, \dots, a_{m} \}$과 $\mathcal{B} = \{ b_{1}, b_{2}, \dots, b_{n} \}$ 가 있을 때, $\mathcal{A}$에서 $\mathcal{B}$로 변환되는 함수를 $T = \mathcal{A} \rightarrow \mathcal{B}$ 로 쓴다. 
-  $T$는 $a \in \mathcal{A}, b \in \mathcal{B}$를 만족하는 $(a,b)$의 집합이다.
- $\mathcal{A}$ 를 _domain_ $\mathcal{B}$를 _range_ 혹은 _codomain_이라고 부른다. 이 때, function $T$내에 모든 쌍은 unique하고 $a \in \mathcal{A}$인 모든 원소는 한 쌍씩이어야만 한다. 
- $a$에 매칭되는 $\mathcal{B}$의 값을 $T(a)$혹은 $aT$로 표기하고 _image of a_라고 부르고 이 때 $a$는 _preimage_라고 한다.
- 모든 _domain_은 쌍에 포함되어야하고 _codomain_은 그렇지 않아도 된다.

![Mapping](/assets/images/post/2019-11-08-Linear-Mapping/Mapping1.jpg)

&nbsp;&nbsp;&nbsp;&nbsp;

### Composition of Mappings
$T = $\mathcal{A} \rightarrow  \mathcal{B}$ 와 $U = $\mathcal{B} \rightarrow  \mathcal{C}$가 있을 때, 모든 $a \in \mathcal{A}$에 대해서 mapping되는 $b \in \mathcal{B}$, $T(a) = b$와 모든 $b \in \mathcal{B}$에 대해서 mapping되는 $c \in \mathcal{C}$, $U(b) = c$가 있다. 이 떄, 이 두 함수 $T$와 $U$를 $a$에 적용하는 것을 $T$와 $U$의 _composition_이라고 하고 

$(U \circ T) (a) = U(T(a))$ 혹은 $a(T \circ U) = aTU$ 라고 표기한다.

Mapping의 Composition은 결합법칙이 성립한다. 예를 들어 $T: \mathcal{A} \rightarrow \mathcal{B}$  $U: \mathcal{B} \rightarrow \mathcal{C}$ $V: \mathcal{C} \rightarrow \mathcal{D}$ 이 있을 때,  $(V \circ U) \circ T = V \circ (U \circ T)$가 성립한다.

&nbsp;&nbsp;&nbsp;&nbsp;

### 특별한 종류의 Mapping
$T : \mathcal{A} \rightarrow \mathcal{B}$이고 $a \in \mathcal{A}$, $b \in \mathcal{B}$ 일 때

* _one to one_ : 모든 $a$가 unique한 $b$에 mapping되는 경우
* _onto_ : 모든 $b$가 몇몇, 혹은 모든 $a$로 인해 mapping되는 경우
* _isomorphic_ : _one to one_이면서 _onto_인 경우

![Special Type of Mapping](/assets/images/post/2019-11-08-Linear-Mapping/special1.jpg)

&nbsp;&nbsp;&nbsp;&nbsp;

### Inverse Mapping
$T : \mathcal{A} \rightarrow \mathcal{B}$에서 $TT^(-1) = I$를 만족하는 $T^(-1) : \mathcal{B} \rightarrow \mathcal{A}$가 존재할 때 _invertible_하다고 한다. 이 때, $T$는 _isomorphic_해야한다.

![Special Type of Mapping](/assets/images/post/2019-11-08-Linear-Mapping/Invertible1.jpg)

&nbsp;&nbsp;&nbsp;&nbsp;

# Linear Mapping
----
두 vector space $\mathcal{A}$와 $\mathcal{B}$에 대해 vector addition과 scalar multiplication을 유지하는 $T = \mathcal{A} \rightarrow \mathcal{B}$를 Linear Mapping이라고 한다.

1. $\forall \vec{u}, \vec{v} \in \mathcal{A}, T(\vec{u} + \vec{v}) = T(\vec{u}) + T(\vec{v})$
2. $\forall \alpha \in \mathbb{R}$ and $\forall \vec{v} \in \mathcal{A}, T(\alpha \vec{v}) = \alpha T(\vec{v})$

이 때, Linear Mapping은 Linear Combination을 유지한다. 즉, $T(\alpha \vec{u} + \beta \vec{v}) = \alpha T(\vec{u}) + \beta T(\vec{v})$ 이다.

_ono to ono_이면서 _onto_인 Linear Mapping을 _isomorphism_ 이라고 한다.


&nbsp;&nbsp;&nbsp;&nbsp;

# Linear Mapping의 Matrix 표현
----
* vector space $A$는 $\vec{u_1}, \vec{u_2), \dots, \vec{u_m}$의 _basis vector_로 이루어져있고 vector space $B$는 $\vec{v_1}, \vec{v_2), \dots, \vec{v_n}$의 _basis vector_로 이루어져있다.
* $T : \mathcal{A} \rightarrow \mathcal{B}$

여기서 $A$의 _basis vector_를 $B$로 transform했을 때, $B$ 내에 있는 $T(\vec{u_1}), T(\vec{u_2}), \dots, T(\vec{u_m})$은 $B$의 _basis vector_를 가지고 다음과 같이 표현할 수 있다.

$T(\vec{u_1}) = a_{1,1}\vec{v_1} + a_{1,2}\vec{v_2} + \dots + a_{1,n}\vec{v_n}$
&nbsp;
$T(\vec{u_2}) = a_{2,1}\vec{v_1} + a_{2,2}\vec{v_2} + \dots + a_{2,n}\vec{v_n}$
&nbsp;
$\vdots$
&nbsp;
$T(\vec{u_m}) = a_{m,1}\vec{v_1} + a_{m,2}\vec{v_2} + \dots + a_{m,n}\vec{v_n}$

여기서 위 식의 coefficients를 matrix $T$로 변경했을 때 다음과 같이 된다.

$$
T = 
\begin{bmatrix} 
a_{1,1} & a_{1,2} & \dots & a_{1,n} \\ 
a_{2,1} & a_{2,2} & \dots & a_{2,n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{m,1} & a_{m,2} & \dots & a_{m,n} 
\end{bmatrix}
$$

이 때, 두 가지 사실을 확인할 수 있다.

* $x \in \mathcal{A}$가 $T$로 인한 transform은 다음과 같이 표시된다.

$$
T(\vec{x}) = 
\begin{bmatrix}
x_1 & x_2 & \dots & x_m
\end{bmatrix}
\begin{bmatrix} 
a_{1,1} & a_{1,2} & \dots & a_{1,n} \\
a_{2,1} & a_{2,2} & \dots & a_{2,n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{m,1} & a_{m,2} & \dots & a_{m,n} 
\end{bmatrix}
$$

* vector space $A$, $B$, $C$가 있을 때, linear mapping $T : A \rightarrow B$, $S : B \rightarrow C$ 라고하자. 여기서 $T$와 $S$의 _combination_은 아래와 같이 표기한다.

$S(T(\vec{v})) = \vec{v}TS$
