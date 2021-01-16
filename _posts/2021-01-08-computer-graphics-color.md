---
title : "[Graphics]컬러 처리"
layout: post
author: Kim heetae
tag : [Graphics, Color]
---
> OpenGL로 배우는 3차원 그래픽스 Chapter 06 내용 정리

## 좌표계
### 3차원 물체의 표현
그래픽에서는 물체 표면만 표현하는 방법이 주로 사용되는데 곡선도 다각형(Polygon)의 집합으로 표현된다. 곡면을 표현하는 평면 다각형 하나하나를 **Mesh, Polygon** 등으로 부른다. 다각형은 여러개의 **정점(Vertex)** 으로 구성된다.

> Mesh는 보통 삼각형으로 표현하는데 그 이유는 삼각형의 점 3개는 무조건 한 평면 위에 있기 때문이다.


### 벡터 공간
* Scalar : 크기만 있고 방향은 없는 양. 교환 법칙, 결합 법칙, 역원 법칙 성립
* Vector : 크기와 방향을 동시에 지닌 것. 역벡터(크기가 같고 방향이 반대) 존재하고, 스칼라의 곱 가능하다. 벡터끼리의 합은 벡터이다.

주어진 벡터로부터 파생되는 모든 벡터의 집합을 **벡터 공간(vector space)** 라고 한다. 

### 어파인 공간
**어파인 공간(Affine Space)** 은 벡터 공간에 **점(Point)** 개념을 넣어서 확장한 공간으로 기하학적인 위치를 표현할 수 있다. 어파인 공간에서는 아래 연산들이 가능하다.
1. 벡터끼리의 덧셈, 뺄셈
2. 스칼라와 벡터간의 곱셈, 나눗셈
3. 점과 벡터의 덧셈, 뺄셈
어파인 공간에서 선분식은 아래와 같다
$$ V = (1-t)P + (t)Q $$
이는 주의해야하는데 어파인공간에서는 점끼리의 덧셈을 허용하지 않는다. 하지만 계수의 합이 1이 된다면 허용을 하는데 이를 **Affine Sum** 이라고 부른다

### 좌표축과 좌표계
원기둥 좌표계, 원구 좌표계, 직교 좌표계 등이 있지만 직교 좌표계를 가장 많이 사용한다. 서로 직각으로 교차하는 3개의 좌표축 벡터로 이루어지는 좌표계를 직교 좌표계라고 부른다. 이 때 좌표축 벡터처럼 자신들의 합성으로 다른 모든 벡터를 표시할 수 있는 벡터를 **기반 벡터(Basis Vector)** 라고 하고 이들은 서로 **Linear Independence** 하다. 그리고 Basis의 갯수를 **Dimension** 이라고 한다.

### 동차좌표
벡터와 점을 구분해 사용하기 위해 3차원의 좌표를 4차원으로 올려서 표현하는 것을 **동차 좌표(Homogeneous Coordinates)** 라 한다.
$v = 4V_1 + 2V_2 + V_3 + 0*r \rightarrow (4,2,1,0)$
$v = 4V_1 + 2V_2 + V_3 + 1*r \rightarrow (4,2,1,1)$

마지막 요소가 0이면 벡터, 그렇지 않으면 점을 의미한다.

<br/>

## 기하 변형
물체의 이동, 회전, 크기 조절 등의 작업을 그래픽스에서는 **기하 변환(Transformation)** 이라고 한다. 모든 종류의 변환이 곱셈으로 표현되어야하고 그래픽 하드웨어도 행렬의 곱셈에 최적화 되어있다.

### 이동 변환(Translatinal Tranformation)
![](/assets/resource/2021-01-16-computer-graphics-modelviewtranform/translate.PNG)
$$
\begin{bmatrix}  x' \\ y' \\ z' \\ 1  \end{bmatrix}  = 
\begin{bmatrix}
  1 & 0 & 0 & T_x \\ 
  0 & 1 & 0 & T_y \\ 
  0 & 0 & 1 & T_z \\ 
  0 & 0 & 0 & 1 \\ 
  \end{bmatrix}
  \begin{bmatrix}  x \\ y \\ z \\ 1  \end{bmatrix}
$$
x,y,z를 $T_x, T_y, T_z$씩 이동하는 행렬이다.

### 회전(Rotation Transformation)
![](/assets/resource/2021-01-16-computer-graphics-modelviewtranform/rotate.PNG)
* Z축을 기준으로 회전

$$
\begin{bmatrix}  x' \\ y' \\ z' \\ 1  \end{bmatrix}  = 
\begin{bmatrix}
  \cos{\theta} & -\sin{\theta} & 0 &0 \\ 
  \sin{\theta} & \cos{\theta} & 0 &0 \\ 
  0 & 0 & 1 & 0 \\ 
  0 & 0 & 0 & 1 \\ 
  \end{bmatrix}
  \begin{bmatrix}  x \\ y \\ z \\ 1  \end{bmatrix}
$$
* X축을 기준으로 회전

$$
\begin{bmatrix}  x' \\ y' \\ z' \\ 1  \end{bmatrix}  = 
\begin{bmatrix}
  1 & 0 & 0 & 0 \\ 
  0 & \cos{\theta} & -\sin{\theta}  & 0\\ 
  0& \sin{\theta} & \cos{\theta} & 0\\ 
  0 & 0 & 0 & 1 \\ 
  \end{bmatrix}
  \begin{bmatrix}  x \\ y \\ z \\ 1  \end{bmatrix}
$$
* Y축을 기준으로 회전

$$
\begin{bmatrix}  x' \\ y' \\ z' \\ 1  \end{bmatrix}  = 
\begin{bmatrix}
  \cos{\theta} & 0 & \sin{\theta} & 0\\ 
    0 & 1 & 0 & 0 \\ 
  -\sin{\theta} & 0 & \cos{\theta} & 0 \\ 
  0 & 0 & 0 & 1 \\ 
  \end{bmatrix}
  \begin{bmatrix}  x \\ y \\ z \\ 1  \end{bmatrix}
$$


### 크기 조절(Scaling Tranformation)
![](/assets/resource/2021-01-16-computer-graphics-modelviewtranform/scale.PNG)
$$
\begin{bmatrix}  x' \\ y' \\ z' \\ 1  \end{bmatrix}  = 
\begin{bmatrix}
  S_x & 0 & 0 & 0\\ 
  0 & S_y & 0 & 0 \\ 
  0 & 0 & S_z & 0 \\ 
  0 & 0 & 0 & 1 \\ 
  \end{bmatrix}
  \begin{bmatrix}  x \\ y \\ z \\ 1  \end{bmatrix}
$$
x,y,,z 축 별로 $S_x, S_y, S_z$ 배만큼 조절하는 행렬로 모든 배율이 같을 때 균등 크기조절, 하나라도 다르면 차등 크기조절 이라고 한다.

### 전단
![](/assets/resource/2021-01-16-computer-graphics-modelviewtranform/shear.PNG)
$$
\begin{bmatrix}  x' \\ y' \\ z' \\ 1  \end{bmatrix}  = 
\begin{bmatrix}
  1 & Sh_y & 0 & 0\\ 
  Sh_x & 1 & 0 & 0 \\ 
  0 & 0 & 1 & 0 \\ 
  0 & 0 & 0 & 1 \\ 
  \end{bmatrix}
  \begin{bmatrix}  x \\ y \\ z \\ 1  \end{bmatrix}
$$
물체를 x-y평면을 따라 전단한 행렬이다.

### 복합 변환
![](/assets/resource/2021-01-16-computer-graphics-modelviewtranform/complex.PNG)
일반적으로 위 변환들은 연속적으로 가해진다. 주의할 점은 작업이 가해진 역순으로 곱해야 한다는 것이다. 예를들어 S1크기번환을 하고 R1 회전을 한 후 S2크기 변환을 하면 $C = S2 * R1 * S1 * P$ 가 된다. 이 때 $C$ 를 복합 행렬이라고 부른다.
복합 변환의 예는 중심점 기준 회전이 있다. 물체의 중심에서 회전을 하고 싶으면 원점으로 이동 시킨 후 회전을 하고 다시 원래 위치로 복구하면 된다.
$$ C = T(X_p,Y_p,Z_p) * R_z(\theta) * T(-X_p,-Y_p,-Z_p) $$

### 반사
![](/assets/resource/2021-01-16-computer-graphics-modelviewtranform/reflection.PNG)
$$
\begin{bmatrix}  x' \\ y' \\ z' \\ 1  \end{bmatrix}  = 
\begin{bmatrix}
  1 & 0 & 0 & 0\\ 
  0 & -1 & 0 & 0 \\ 
  0 & 0 & 1 & 0 \\ 
  0 & 0 & 0 & 1 \\ 
  \end{bmatrix}
  \begin{bmatrix}  x \\ y \\ z \\ 1  \end{bmatrix}
$$
x-z평면을 기준으로 반사하는 행렬이다.

### 변환의 분류
* 강체변환(RigidBody Transformation) : 변환 전 후 내부 정점 간 거리가 유지(이동, 회전)
* 유사변환(Similarity Transformation) : 물체 면 사이의 각이 유지되고 내부 정점간 거리 비율이 유지(강체변환 + 균등 크기 조절 변환)
* 어파인 변환(Affine Transformation) : 물체의 타입이 유지됨. 즉, 직선은 직선으로 곡면은 곡면으로 변환됨. 그리고 평행한 선분은 평행하게 유지됨(유사변환 + 차등 크기 조절 변환 + 전단 변환)
* 원근 변환(Perspective Transformation) : 물체의 타입만 유지됨
* 어파인변환 + 원근변환 = 선형 변환(Linear Transformation)
![](/assets/resource/2021-01-16-computer-graphics-modelviewtranform/transformation.PNG)

<br/>

## 지엘의 모델 변환
### 모델 좌표계와 전역 좌표계
### 지엘 파이프라인
### 모델 변환
### 복합 변환에 의한 모델링
### 행렬 스택 활용
### 계층 구조 모델링

<br/>

## 지엘의 시점 변환
### 시점 좌표계 설정
### 지엘의 시점 좌표계

<br/>
