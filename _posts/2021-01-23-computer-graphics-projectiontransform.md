---
title : "[Graphics]투상 변환과 뷰 포트 변환"
layout: post
author: Kim heetae
mathjax: true
tag : [Graphics, Transfrom]
---
> OpenGL로 배우는 3차원 그래픽스 Chapter 07 내용 정리

## 투상
**가시 변환(Viewing Transformation)** 은 3차원 물체를 화면으로 사상하기 위한 작업이다.
* COP(Center of Projection) : 시점 좌표계의 원점, 투상 중심
* Projectors : 투상선
* Line of Sign : 카메라가 바라보는 방향, 시선
* View Plane : 물체 영상이 맺히는 투상면

으로 이루어져 있다.

![](/assets/resource/2021-01-16-computer-graphics-projectiontransform/projection.PNG)

### 평행 투상
투상선이 나란한 투상법. 거리와 무관하게 같은 길이로 투상된다.

![](/assets/resource/2021-01-16-computer-graphics-projectiontransform/orthographic.PNG)

* 정사 투상
투상선과 투상면이 직교하고 주 평면중 하나와 투상면이 나란하다. 평면도, 입면도, 측면도 등에 사용

* 축측 투상
투상선과 투상면이 직교하지만 주 평면과 투상면이 나란하지 않아도 된다. 실제 길이가 보존되지 않는다.

* 경사 투상
투상선이 나란하지만 투상선과 투상면이 직교하지 않는다.

### 원근 투상
시점과 물체가 유한한 거리에 있다고 간주하고 투상선이 시점으로부터 방사선 모양으로 퍼져나가는 투상법. 실제 카메라나 눈에 적합하고 원근감이 느껴진다.
![](/assets/resource/2021-01-16-computer-graphics-projectiontransform/perspective.PNG)

> 어파인 변환과 원근 변환의 공통점은 각,거리가 보존되지 않는 것이다. 다만 원근 변환은 물체 정점간 거리에 대한 축소율이 달라진다.

## 지엘의 투상 변환
지엘에서 투상을 사용하기 위해서는 행렬 모드를 투상 행렬로 바꿔주어야한다.

```c++
void glMatrixMode(GL_PROJECTION);
```

GL_MODELVIEW은 물체에 대한 기하 변환, 카메라 위치 및 방향에 대한 기하 변환이고 GL_PROJECTION은 촬영 방법 및 렌즈 선택에 대한 기하 변환이다

현 투상행렬 값을 검색하는 법은 아래와 같다.
```c++
void glGetFloatv(GLenum pname, GLfloat *params); 

glGetFloatv(GL_PROJECTION_MATRIX, MyMatrix); //투상행렬의 현 투상행렬값 검색
glGetFloatv(GL_MODELVIEW_MATRIX, MyMatrix); //모델뷰행렬의 현 투상행렬값 검색
```

### 지엘의 평행 투상
* 기본 투상

카메라는 항상 -z를 바라본다. 투상면이 d만큼 떨어진 거리에 있고 점 $P$ 가 있다고 할 때 투상면에 점 $P^{'}$ 은 아래와 같다.
$$
\begin{bmatrix}  x' \\ y' \\ z' \\ 1  \end{bmatrix}  = 
\begin{bmatrix}
  1 & 0 & 0 & 0\\ 
  0 & 1 & 0 & 0 \\ 
  0 & 0 & 0 & -d \\ 
  0 & 0 & 0 & 1 \\ 
  \end{bmatrix}
  \begin{bmatrix}  x \\ y \\ z \\ 1  \end{bmatrix} = 
    \begin{bmatrix}  x \\ y \\ -d \\ 1  \end{bmatrix}
$$

> 이렇게 하면 z값이 모두 같아 Depth를 알 수 없게 되기 때문에 지엘에서는 3차원 좌표를 유지한다.

* 정규화 가시 부피에 의한 투상

![](/assets/resource/2021-01-16-computer-graphics-projectiontransform/viewplane.PNG)

투상 범위를 제한해야 해서 **가시 부피(View Volume)** 이라는 투상 범위를 정한다. 이 때, 시점과 가까운 쪽을 Near Clipping Plane, 먼 쪽을 Far Clipping Plane 이라고 한다. 가시 부피 밖에 있으면 절단(Clipping) 된다.

지엘에서 가시 부피는 아래와 같이 정할 수 있다.

```c++
void glOrtho(GLdouble left, GLdouble right, GLdouble bottom, GLdouble top, GLdouble near, GLdouble far);
// 가시 부피의 상하좌우전후를 정함
// 이름은 Ortho지만 실제로는 정사 투상이 아니고 일반적인 평행 투상임
```
지엘에서는 가시 부피를 가로,세로,높이가 2인 정육면체로 **정규화 변환(Normalization Transformation)** 해서 사용한다.

![](/assets/resource/2021-01-16-computer-graphics-projectiontransform/normalizationtransform.PNG)

이 결과를 **정규화 가시 부피(CCV : Canonical View Volume)** 이라고 한다.  정규화 가시 부피의 주의할 점은 좌표계를 왼손 법칙을 사용한다는 것이다.

> 가시 부피를 그대로 사용하지 않고 정규화 가시 부피를 사용하는 이유
> 1. 평행 투상, 원근 투상 모두 정규화 가시 부피를 사용해 파이프라인 구조 동일화
> 2. 물체 절단하는 것에 있어서 정규화 가시 부피를 이용하는 것이 용이
> 3. 화면 좌표계로 변환이 수월해짐

정규화 변환 행렬은 다음과 같다

 - $T$ : 시점 좌표계 원점으로 이동 
 - $S$ : 크기 조절 
 - $R$ : z축 방향이 바뀌므로 반사 변환


$$
N = R \cdot S \cdot T   
$$

이로 인해 바뀐 좌표계를 **절단 좌표계(CCS : Clip Coordinate System)** 이라고 부른다

> 마찬가지로 이 연산은 z값이 모두 같아 Depth를 알 수 없게 되기 때문에 지엘에서는 이 과정을 나중으로 미뤄서 3차원 좌표를 유지한다.

### 지엘의 원근 투상
* 기본 투상
원근 투상의 측면도는 이와 같아서 아래 수식으로 표현된다.

![](/assets/resource/2021-01-16-computer-graphics-projectiontransform/perspectiveside.PNG)

$$
P^{'} = \begin{bmatrix}
  1 & 0 & 0 & 0\\ 
  0 & 1 & 0 & 0 \\ 
  0 & 0 & -1 & 0 \\ 
  0 & 0 & 1/d & 0 \\ 
  \end{bmatrix}
  \begin{bmatrix}  x \\ y \\ z \\ 1  \end{bmatrix} = 
    \begin{bmatrix}  x \\ y \\ -z \\ z/d  \end{bmatrix}
$$

이 식에서 마지막 요소를 1로 바꾸는 **원근 분할(Perspective Division, Homogenization)** 을 하게 되면

$$
P^{'} = 
    \begin{bmatrix}  \frac{x}{z/d} \\ \frac{y}{z/d} \\ -d \\ 1  \end{bmatrix}
$$

> $M_{perspective}$ 의 값이 (0,0,0,1)이 아니므로 Affine Transformation이 아니다


```c++
void glFrustum(GLdouble left, GLdouble right, GLdouble bottom, GLdouble top, GLdouble near, GLdouble far);
```

* 정규화 가시 부피에 의한 투상

![](/assets/resource/2021-01-16-computer-graphics-projectiontransform/normalizedperspective.PNG)

사각뿔에서 전방 절단면과 후방 절단면에 의해 잘린 **절두체(Frustum)** 

이를 평행 투상과 동일하게 정규화 가시 부피로 변환한다.

정규화 변환 행렬은 다음과 같다

 - $Sh$ :  대칭적인 모습을 만들기 위한 Shear 변환
 - $S$ : 절단면 가로 세로 길이 2n이 되도록 고정시키는 변환
 - $T$ : 절두체 가시 부피를 정규화 육면체로 바꾸는 변환행렬

$$
N = T \cdot S \cdot Sh  \\
$$

원근 변환에 따른 간격 변화는 옆에서 보면 다음과 같아서 물체간 간격이 비 선형임을 알 수 있다.

![](/assets/resource/2021-01-16-computer-graphics-projectiontransform/perspectiveforeshortening1.PNG)

![](/assets/resource/2021-01-16-computer-graphics-projectiontransform/perspectiveforeshortening2.PNG)

아래 함수로 더 손쉽게 선언할 수 있다

```c++
void gluPerspective(GLdouble fov, Gldouble aspect, GLdouble near, GLdouble far);
```

### 투상 파이프라인

![](/assets/resource/2021-01-16-computer-graphics-projectiontransform/glcoordinatepipeline.PNG)


## 지엘의 뷰 포트 변환
원근 분할의 결과 좌표계를 **정규화 장치 좌표계(NDCS : Normalized Device Coordinate System)** 라 한다. 정규화 장치 좌표계에서 화면 좌표계로 가는 변환을 **뷰 포트 변환(Viewport Transformation)** 이라고 한다. 화면 좌표계를 **뷰 포트 좌표계(Viewport Coordinate System)** 또는 **윈도우 좌표계(Window Coordinate System)** 라고도 부른다.

### 뷰 포트 설정

```c++
void glViewport(GLint left, GLint bottom, GLsizei width, GLsizei height);
//뷰 포트의 위치 및 크기를 지정하는 함수
```
위 함수를 사용하면  정규화 장치 좌표계의 정점이 뷰 포트 좌표계로 변환된다.

화면 좌표계는 화소 단위므로 위에서 계산된 값을 정수로 변환해야 하는데 이를 **래스터 변환(Rasterization)** 이라고 한다. 

z값을 [-1, 1] 에서 [0, 1]로 **재 정규화(Renormalization)** 한다.

$$
P^{'}  = 
    \begin{bmatrix}  x^{'} \\ y^{'} \\ z^{'} \\ 1  \end{bmatrix}= 
    \begin{bmatrix}
  \frac{width}{2} & 0 & 0 & left + \frac{width}{2}\\ 
  0 & \frac{height}{2} & 0 & bottom + \frac{height}{2} \\ 
  0 & 0 & \frac{1}{2} & \frac{1}{2} \\ 
  0 & 0 & 0 & 1 \\ 
  \end{bmatrix}
  \begin{bmatrix}  x \\ y \\ z \\ 1  \end{bmatrix}
$$
