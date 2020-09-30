---
title: "[Linear Algebra]Affine Transformation"
layout: post
date: 2019-12-27 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Graphics
- Linear Algebra
- Affine Transformation
category: blog
author: heetaekim
description: Affine Transformation
MathJax: true
---
# Affine Transformation
---
_Affine Transformation_ 은 한 Affine Space에 있는 point와 vector를 다른 Affine Space에 point와 vector에 Mapping하는 것이다. 
일반적으로 아래와 같이 유지가 되면  $T:\mathcal{A}^n\mapsto\mathcal{B}^m$ 의 식을 affine transformation이라고 한다.
$$T(a_1P_1 + a_2P_2 + \dotsi + a_nP_n) = a_1T(P_1) + a_2(T_2) + \dotsi + a_nT(P_n)$$
$$P_i \in \mathcal{A}, \ \sum^n_{i=1}a_i = 1, \ m \leq n$$

### 상대적 비율 유지
Affine Transformation은 상대적 비율을 보존한다. 아래에서 살펴보자.
다음과 같은 Affine Combination이 있다고 하자.

$$R = (1-\alpha)P + \alpha Q$$
이 식에 Affine Map $T$를 적용하면
$$
\begin{aligned}
    T(R) & = T((1-\alpha)P + \alpha Q) \\
         & = (1-\alpha)T(P) + \alpha T(Q) 
\end{aligned}
$$

이는 parametric form으로 적용할 수 있다
$$R(t) = (1 - t)P + tQ$$

이 식에 $T$를 적용하면

$$
\begin{aligned}
    T(R(t)) & = T((1-t)P + tQ) \\
         & = (1-t)T(P) + tT(Q) 
\end{aligned}
$$

위 식에서 방정식의 상수는 Transformation $T$의 영향을 받지 않아서 t와 1 - t는 변하지 않는다. 따라서 $R, P, Q$의 상대적 거리는 동일하다.

### 평행성 유지
Affine Transformation은 평행성도 유지한다.
${P_1, P_2}$와 ${Q_1,Q_2}$ 가 있을 때 각 점들로 line을 정의할 수 있다.
$$
\begin{aligned}
    L_1 = P_1 + \alpha(P_2 - P_1) \\
    L_2 = Q_1 + \beta(Q_2 - Q_1)
\end{aligned}
$$

이 때 $P_2 - P_1 = \gamma(Q_2 - Q_1)$이라면 두 선은 평행할 것이다. vector는 $\vec{v} = Q - P$로 정의되는데 여기에 Affine Tranformation을 적용하면 
$$
\begin{aligned}
    
T(\vec{v}) & = T(Q - P) \\
        & = T(Q) - T(P)
\end{aligned}
$$

이므로 Affine Transformation은 평행성에 영향을 미치지 않는다. 즉, 평행성을 유지하게 된다.

### Affine Coordinate
1. $T(\vec{u} + \vec{v}) = T(\vec{u} + T(\vec{v})$
2. $T(\alpha\vec{v}) = \alpha T(\vec{v})$
3. $T(P + \vec{v}) = T(P) + T(\vec{v})$

위 세가지 법칙으로 아래 식도 만족하게 된다.

$$T(\alpha_1\vec{v}_1 + \alpha_2\vec{v}_2 + \dotsi + \alpha_n\vec{v}_n + \mathcal{O}) = \alpha_1 T(\vec{v}_1) + \alpha_2 T(\vec{v}_2) + \dotsi + \alpha_n T(\vec{v}_n) + T(\mathcal{O})$$

즉 Affine Coordinate를 유지한다.