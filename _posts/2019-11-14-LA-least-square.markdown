---
title: "[Linear Algebra]Least Sqaure"
layout: post
date: 2019-11-14 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Graphics
- Linear Algebra
- Least Sqaure
category: blog
author: heetaekim
description: Least Sqaure
MathJax: true
---
# Least Sqaure
----
우리는 Linear System이 유일한 해를 갖을 때, 쉽게 해를 구할 수 있다. 예를 들어 Point 2개를 지나는 직선을 구하는 Linear System을 만들어보자. 

$P_{1} = (p_{1,1}, p_{1,2})$
&nbsp;&nbsp;&nbsp;&nbsp;
$P_{2} = (p_{2,1}, p_{2,2})$
&nbsp;&nbsp;&nbsp;&nbsp;

이 두 점을 지나는 직선을 $y = mx + b$ 라고 했을 때, m과 b를 coefficients로 갖는 Linear System은 다음과 같다.

$p_{1,1}m + b = p_{1,2}$
&nbsp;&nbsp;&nbsp;&nbsp;
$p_{2,1}m + b = p_{2,2}$
&nbsp;&nbsp;&nbsp;&nbsp;

즉,

$$
 \begin{bmatrix} 
 p_{1,1}  &  1  \\
 p_{2,1}  &  1  \\  
 \end{bmatrix} 
 
 \begin{bmatrix} 
 m \\
 b \\ 
 \end{bmatrix} 
=
\begin{bmatrix} 
 p_{1,2} \\
 p_{2,2} \\ 
 \end{bmatrix} 
$$

이다.

이 Linear System의 해를 구하면

$$
m = \frac{p_{1,2} - p_{2,2}}{p_{1,1} - p_{2,1}} , b = \frac{p_{1,1}p_{2,2} - p_{2,1}p_{1,2}}{p_{1,1} - p_{2,1}}
$$

이다.

그림으로 표현하면 아래와 같다.

![2개의 점을 지나는 직선](/assets/images/post/2019-11-14-Least-Sqaure/twopoint.jpg)

하지만 여기서 점 1개가 추가된다면 Linear System은 아래와 같아진다.

$$
 \begin{bmatrix} 
 p_{1,1}  &  1  \\
 p_{2,1}  &  1  \\  
 p_{3,1}  &  1  \\  
 \end{bmatrix} 
 
 \begin{bmatrix} 
 m \\
 b \\ 
 \end{bmatrix} 
=
\begin{bmatrix} 
 p_{1,2} \\
 p_{2,2} \\ 
 p_{3,2} \\ 
 \end{bmatrix} 
$$

이러한 경우를 _overdetermined_ 한 system이라고 부르는데 해가 없을 수 도 있다.

![3개의 점을 지나는 직선](/assets/images/post/2019-11-14-Least-Sqaure/threepoint.jpg)

이 때 우리는 세 점을 최대한 만족하는 직선을 구할 것이다. 이 때 사용되는 방법이 Least Sqaure(최소자승법) 이다.

&nbsp;&nbsp;&nbsp;&nbsp;

# 정의
---
세 점을 최대한 만족하는 직선을 구할 떄에는 기준이 필요할 것이다. Least Sqaure에서는 최종 직선과 세 점간의 y값들의 차를 기준으로 한다. 세 점을 예시로 들었지만 이 후에 나오는 예시에서의 점의 갯수는 $n$개라고 하겠다. 한 점과 직선의 y값의 차($D$)는 $|f(x) - y|$ 이다. 우리는 이 값들의 제곱들의 합이 최소가 되는 m과 b를 구할 것이다. 이 것이 Least Sqaure라고 불리는 이유이다. 따라서 공식은

$$
D^2 = (f(x_1) - y_1)^2 + (f(x_2) - y_2)^2 + \dotsb + (f(x_n) - y_n)^2
$$

로 세울 수 있다. 

&nbsp;&nbsp;&nbsp;&nbsp;

# 풀이
----

이 때 우리가 구하려는 식 $y = mx + b$를 위 공식에 대입해보면 아래와 같을 것이다.

$$
D^2 = (b + mx_1 - y_1)^2 + (b + mx_2 - y_2)^2 + \dotsb + (b + mx_n - y_n)^2
$$

이 때, $D^2$의 최소값을 구하기 위해서는 $D^2$이 $m,b$ 좌표평면에 있을 때 m과 b에 대해서 편미분한 값이 0이 되는 지점이 최소값이 될 것이다. 그러면 아래와 같은 식을 도출할 수 있다.

$$
0 = (b + mx_1 - y_1) + (b + mx_2 - y_2) + \dotsb + (b + mx_n - y_n)
$$

$$
0 = x_1(b + mx_1 - y_1) + x_2(b + mx_2 - y_2) + \dotsb + x_n(b + mx_n - y_n)
$$

위 두개의 식으로 Linear System을 세우면

$$
 \begin{bmatrix} 
 1  &  1  & \dots & 1\\
 x_1 & x_2 & \dots & x_n  \\  
 \end{bmatrix} 
 (
 \begin{bmatrix} 
 1 & x_1\\
 1 & x_2\\
 \vdots \\
 1 & x_n\\ 
 \end{bmatrix} 
 \begin{bmatrix} 
 b\\
 m\\
 \end{bmatrix} 
-
\begin{bmatrix} 
 y_1 \\
 y_2 \\ 
 \vdots \\ 
 y_n \\ 
 \end{bmatrix} 
 )
=
\begin{bmatrix} 
 0 \\
 0 \\ 
 \end{bmatrix} 
$$

이 때 $x_n$들이 최종 직선으로 Transformation되는 것을 나타내는 matrix $M$은 다음과 같이 표현할 수 있다.

$$
M = 
 \begin{bmatrix} 
 1 & x_1\\
 1 & x_2\\
 \vdots \\
 1 & x_n\\ 
 \end{bmatrix} 
$$

$M$을 위 최종 식에 대입한 후 식을 풀어보겠다.


$$
 M^T
 (
 M
 \begin{bmatrix} 
 b\\
 m\\
 \end{bmatrix} 
-
\begin{bmatrix} 
 y_1 \\
 y_2 \\ 
 \vdots \\ 
 y_n \\ 
 \end{bmatrix} 
 )
=
\begin{bmatrix} 
 0 \\
 0 \\ 
 \end{bmatrix} 
$$

&nbsp;&nbsp;&nbsp;&nbsp;

$$
 M^T
 (
 M
 \begin{bmatrix} 
 f
 \end{bmatrix} 
-
\begin{bmatrix} 
 y
 \end{bmatrix} 
 )
=
0
$$

&nbsp;&nbsp;&nbsp;&nbsp;

$$
 M^T
 (
 M
 \begin{bmatrix} 
 f
 \end{bmatrix} 
-
\begin{bmatrix} 
 y
 \end{bmatrix} 
 )
=
0
$$

&nbsp;&nbsp;&nbsp;&nbsp;


$$
\begin{bmatrix}
    f
\end{bmatrix}
=
(M^TM)^{-1}M^T
\begin{bmatrix}
    y
\end{bmatrix}
$$

위와 같이 식이 정리가 된다.


&nbsp;&nbsp;&nbsp;&nbsp;

# 적용
----
실제로 세 점 $(1,2), (3,7), (5,6)$에 Least Square를 적용해보겠다.

$$ 
M = 
\begin{bmatrix}
    1 & 1 \\
    1 & 3 \\ 
    1 & 5 \\
\end{bmatrix}
,
M^T = 
\begin{bmatrix}
    1 & 1 & 1 \\
    1 & 3 & 5 \\ 
\end{bmatrix}
$$

가 될 것이고 이를 곱한 후 역행렬을 구하면

$$
(MM^T)^-1 = 
\begin{bmatrix}
    35/24 & -3/8 \\
    -3/8 & 1/8 \\ 
\end{bmatrix}
$$

이 될 것이다. 여기에 $M^T$와 $y$ vector를 연달아 곱해주면 해는
$$
\begin{bmatrix}
    b \\
    m \\ 
\end{bmatrix}
 = 
\begin{bmatrix}
    2 \\
    1 \\ 
\end{bmatrix}
$$
이 나올 것이다. 그러면 직선은 $y = x + 2$라는 결론이 난다.


# 결론
----
Least Sqaure는 많은 부분에서 활용된다. 정확히 기억하진 않지만 예전에 Seamless한 Texture를 만드는 과제를 할 때 Mesh의 UV를 Auto Unwraping 해주는 알고리즘이 필요해서 논문을 찾아본 적이 있는데 이 때 Least Sqaure가 사용되는 것을 본 적이 있다. 또 웹을 통해 공부를 하면서 머신러닝, 영상처리 등에 많이 사용된다는 것도 알게 되었다. 학교에 다닐 때 선형대수2라는 수학과 수업을 따로 들었었는데 그 때 더 자세하게 학습했었는데 다시 정리하려나 기억이 잘 나지 않았다. 이 번 정리를 통해 학교 수업때 만큼은 아니지만 나름대로 Least Square에 대해 다시금 정리할 수 있는 기회가 되었다.
