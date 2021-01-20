---
title : "[Graphics]모델 변환과 시점 변환"
layout: post
author: Kim heetae
mathjax: true
tag : [Graphics, Transfrom]
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

$$v = 4V_1 + 2V_2 + V_3 + 0*r \rightarrow (4,2,1,0)$$
$$v = 4V_1 + 2V_2 + V_3 + 1*r \rightarrow (4,2,1,1)$$

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
* 모델 좌표계(Modeling Coordinate System) : 물체별로 설계상의 편의를 위주로 설정된 좌표계, 지역 좌표계(Local Coordinate System)이라고도 한다
* 전역 좌표계(World Coordinate System) : 동일한 위치를 서로 다른 좌표로 부르지 않기 위해서 장면 안에 모든 물체를 한꺼번에 아우를 수 있는 좌표계
* 시점 좌표계(View Coordinate System) : 시점을 기준으로 정의되는 좌표계
 
![](/assets/resource/2021-01-16-computer-graphics-modelviewtranform/localworldviewcoordinate.PNG)

> 지엘에서는 물체의 이동을 좌표계의 이동으로 정의한다. 변환 행렬은 전역좌표계를 모델좌표계로 일치시키기 위한 것

### 지엘 파이프라인
![](/assets/resource/2021-01-16-computer-graphics-modelviewtranform/glpipeline.PNG)

* Modeling Tranformation : 물체에 대해 이동, 회전, 크기 등 기하 변환을 하는 작업
* View Transformation : 변환된 물체를 관찰하기 위해 카메라를 이동하고 회전해서 설정하는 작업
* Projection Transformation : 카메라 렌즈를 선택하고 물체를 2차원으로 변환하는 작업
* Viewport Transformation : 변환된 2차원 이미지를 줄이거나 늘리는 작업

지엘의 모든 변환은 Transformation Matrix로 대변된다.  지엘에서는 Modeling Transformation과 View Transformation을 합쳐서 **Model-View Trnaformation** 이라고 부르는데 이 때의 행렬을 ModelView Matrix라고 한다.

### 모델 변환
지엘에는 변환의 종류별로 행렬이 따로 존재하는데 모델 뷰 행렬, 투상 행렬, 텍스쳐 행렬이 있다. 이를 선택하기 위해서는 **행렬 모드(Matrix Mode)** 를 설정해야한다. 들어갈 수 있는 상수로는 GL_MODELVIEW, GL_PROJECTION, GL_TEXTURE 가 있다.

```c++
void glMatrixMode(GLenum mode);
```

행렬 모드를 정하고 원하는 변환을 하게 되면 행렬 모드에 해당하는 스택에 쌓이게 된다. **현 변환 행렬(CTM : Current Transformation Matrix)** 가 스택의 행렬들의 값이 된다. 처음에는 CTM을 초기화하고 그 이후 원하는 값을 곱한다
```c++
void glLoadIdentity(); // 현 변환 행렬을 항등 행렬로 초기화한다.
void glLoadMatrixf(const GLfloat *M); // 행렬을 현 변환 행렬로 바꾼다.
void glMultMatrixf(const GLfloat *M);	// 현 변환 행렬에 입력값을 곱한다.
```

위 함수보다 편하게 값을 추가할 수 있는 방법은 아래와 같다.
```c++
void glTranslatef(GLfloat dx, GLfloat dy, GLfloat dz); 
void glScalef(GLfloat sx, GLfloat sy, GLfloat sz); 
void glRotatef(GLfloat angle, GLfloat x, GLfloat y, GLfloat z);	
```


### 복합 변환에 의한 모델링
아래 코드는 2가지 관점에서 생각할 수 있다
```c++
glMatrixMode(GL_MODELVIEW);
glLoadIdentity();
glRotatef(45,0.0,.0.,1.0);
glTranslatef(10.0, 0.0, 0.0);
glVertex3f(Px,Py,Pz);
```
* 물체의 변환 : 고정된 전역 좌표계를 기준으로 물체를 움직임. 물체가 이미 전역좌표계에 있다고 가정하고 전역좌표계 기준으로 변환

![](/assets/resource/2021-01-16-computer-graphics-modelviewtranform/matrixoperation1.PNG)

* 좌표계의 변환 : 모델 좌표계를 변환. 모델좌표계를 변환한 후에 모델좌표계 기준으로 물체를 그리지만 모델좌표계에 변환을 가함

![](/assets/resource/2021-01-16-computer-graphics-modelviewtranform/matrixoperation2.PNG)


### 행렬 스택 활용
지엘은 단순 행렬을 하나만 제공하는 것이 아니고 **행렬 스택(Matrix Stack)** 을 제공한다.
```c++
void glPushMatrix();
void glPopMatrix();
```
현 변환 행렬은 항상 위에 있고 푸시가 될 때마다 복사되어 스택에 쌓이게 된다. 예시 과정은 아래 그림과 같다.
![](/assets/resource/2021-01-16-computer-graphics-modelviewtranform/matrixstack.PNG)


### 계층 구조 모델링
사람 관절이나 태양계처럼 물체(Object)간의 상하 관계를 **계층 구조** 로 설정할 때가 있다. 이 구조는 위에서 언급한 행렬 스택을 이용해 구현할 수 있다.
* Forward Kinematics(FK) : 계층 구조의 상위에서 하위로 내려오며 각도를 회전해 원하는 자세를 만드는 방법.
* Inverse Kinematics(IK) : 계층 구조의 최하위에 있는 물체 위치를 명시하면 상위 계층의 움직임을 자동으로 계산하는 방법.  IK의 해는 여러개가 될 수 있으므로 **제약 조건(Constrains)** 를 걸어둘 수 있다.


<br/>

## 지엘의 시점 변환
시점은 물체를 바라보는 위치로 카메라 위치와 동일하다. 모델 좌표계에서 전역 좌표계로 변환 시 모델 행렬을 곱하는 것처럼 전역 좌표계에서 시점 좌표계로 변환 시 뷰 행렬을 곱하면 되지만 지엘에서는 모델 행렬과 뷰 행렬을 하나로 취급하여 모델 뷰 행렬이라고 표현한다.

### 시점 좌표계 설정
PHIGS 시점 좌표계 : 투상면에 수직인 벡터 VPN(View Plane Normal)와 카메라 위쪽을 향하는 벡터(Veiw Up Vector) 와 시점 좌표계의 원점 VRP(View Reference Point) 등으로 정의 된다.

### 지엘의 시점 좌표계
PHIGS보다 제약되었지만 더 직관적인 인터페이스를 제공한다.

_지엘의 시점좌표계의 3요소_
* 카메라 위치
* 초점의 위치
* 카메라의 기울임

> 카눕혀서 찍을 수도 있지만 세워서 찍을 수도 있기 때문에 카메라의 기울임도 고려해야 한다.

```c++
void gluLookAt(GLdouble eyex, GLdouble eyey, GLdouble eyez, GLdouble atx, GLdouble aty, GLdouble atz, GLdouble upx, GLdouble upy, GLdoubpe upz);
```
카메라 위치(eyex, eyey, eyez)와 초점(atx, aty, atz)와 기울임이 되는 up vector(upx, upy, upz)를 넣어서 카메라를 정의한다.

카메라 위치를 정의하면 시점 좌표계로 변환되므로 아래와 같은 관계가 성립된다.

$$P_{wcs} = M \cdot P_{mcs}$$

$$P_{vcs} = V \cdot P_{wcs} = V \cdot M \cdot P_{mcs}$$

실제로 사용할 때에는 모델 뷰 행렬은 하나로 정의되므로 행렬을 초기화 한 직 후에 정의해야한다.

```c++
glMatrixMode(GL_MODELVIEW);
glLoadIdentity(); // 초기화
gluLookAt(0.2, 0.0, 0.0, 0.0, 0.0, -100.0, 1.0, 1.0, 0.0); // 뷰 행렬
glRotatef(45, 0.0, 1.0, 0.0); // 모델 행렬
glutWireCube(1.0);
```

<br/>
