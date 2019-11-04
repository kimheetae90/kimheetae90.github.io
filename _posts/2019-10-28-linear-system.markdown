---
title: "Square Matrix의 종류"
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
# Square Matrix
----


Square Matrix는 N by N Matrix, 즉 행과 열의 갯수가 같은 Matrix로 선형대수에 있어서 중요한 요소이다. 왜냐하면 Linear System을 표현하거나 다루는 과정 혹은 해를 구할 때 주요 부분을 차지하기 때문이다. Graphics에서는 Square Matrix를 통해 Linear Transform 등 기하 정보를 표현한다. 이 포스팅에서는 몇가지 종류의 Square Matrix에 대해 알아보도록 하자.

&nbsp;&nbsp;&nbsp;&nbsp;
## Diagonal Matrix
----
Diagonal Matrix는 Diagonal(주대각선)을 제외한 나머지 element들은 0인 Matrix를 지칭한다. 이 떄 Diagonal은 0일수도, 아닐수도 있다. 

![Square Matrix Example](/assets/images/post/2019-11-04-Square-Matrix/SquareMatrix.jpg)

Diagonal Matrix의 특징은 다음과 같다.
1. A와 B가 Diagonal Matrix라면 C = AB인 Matrix C는 Diagonal이다.
2. A와 B가 Diagonal Matrix라면 C = AB = BA이다. 즉, 교환법칙이 성립한다.
3. A가 Diagonal Matirx이고 B는 일반 Matrix라면 C = AB의 i번째 행은 B의 i번째 행에 A의 i,i 요소를 곱한 것 과 같다. 열을 계산할 때도 같다.

### Scalar Matrix
Diagonal Matrix중 Diagonal이 모두 같은 Matrix이다.

### Identity Matrix
Scalar Matrix 중 Diagonal이 모두 1인 Matrix이다. 단위행렬이라고 부른다. I라고 표기하고 Matrix 곱의 항등원이 된다. 즉, MI = IM = M 이다.

## Triangular Matrix
----
Diagonal을 기준으로 아래 모든 요소가 0인 Matrix를 Upper Triangular Matrix라고 하고 아래가 모두 0인 Matrix를 Lower Triangular Matrix라고 한다. 그리고 여기서 Diagonal까지 0인 Matrix를 Strict Triangular Matrix라고 한다.

Triangular Matrix의 성질은 다음과 같다.
1. A와 B가 Lower Triangular Matrix라면 C = AB는 Lower Triangular Matrix이고 Upper인 경우도 마찬가지이다.
2. A와 B가 Lower Triangular Matrix라면 C = A + B는 Lower Triangular Matrix이고 Upper인 경우도 마찬가지이다.
3. A가 Invertible한 Lower Triangular Matrix라면 A의 역행렬도 Lower Triangular Matrix이고 Upper인 경우도 마찬가지이다.
4. Triangular Matrix의 Determinant는 Diagonal의 곱이다.

일반 Matrix를 Triangular Matrix들의 곱으로 나타내는 방법을 LU Decomposition이라고 한다.