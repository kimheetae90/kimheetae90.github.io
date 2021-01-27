---
title : "[Graphics]가시성 판단"
layout: post
author: Kim heetae
mathjax: true
tag : [Graphics, Transfrom]
---
> OpenGL로 배우는 3차원 그래픽스 Chapter 08 내용 정리

## 벡터
x축 단위벡터를$i$ , y축 단위벡터를 $j$ 라 하면 점 p의 위치는 다음과 같이 표기할 수 있다.
$$p = 4i + 2j$$
이 때 $i,j$ 를 단위벡터라고 한다.

### 정규화 벡터
원래 벡터와 방향은 동일하지만 크기가 1인 벡터를 정규화 벡터 라고 한다.
$$
p^{'} = (\frac{x}{\vert{p}\vert} , \frac{y}{\vert{p}\vert}, \frac{z}{\vert{p}\vert}) 
\\
 = (\frac{x}{\sqrt{x^{2} + y^{2} + z^{2}}} , \frac{y}{\sqrt{x^{2} + y^{2} + z^{2}}}, \frac{z}{\sqrt{x^{2} + y^{2} + z^{2}}})
$$

### 벡터의 내적과 외적
* 내적 : 벡터 간 방향을 얻고 싶을 때 주로 사용되고 결과는 스칼라값으로 나온다.

 $$s \cdot t = \vert s \vert \vert t \vert \cos{\theta} = s_{x}t_{x} + s_{y}t_{y} + s_{z}t_{z}$$

* 외적 : 평면에 수직인 벡터를 법선 벡터라고 하는데 n은 s와 t가 이루는 평면의 법선벡터이다. 외적의 결과는 벡터이며 외적벡터의 크기는 평행사변형의 넓이에 해당한다.

$$ s \times t = \vert s \vert \vert t \vert \sin{\theta} n
\\ = (s_y t_z - s_z t_y , -s_x t_z + s_z t_x, s_x t_y - s_y t_x)$$


### 평면 표현
평면의 법선벡터가 $(A,B,C)$ 라 할 때 평면은 아래와 같이 표현할 수 있다.
$$Ax + By + Cz + D = 0$$
이 때 D 값에 따라서 여러 개의 나란한 평면이 생성될 수 있다.

### 지엘의 법선 벡터
![](/assets/resource/2021-01-25-computer-graphics-visibility/normalvectorccw.png)

정점을 명시하는 순서에 따라 법선 벡터의 방향이 좌우된다. 지엘에서는 **반시계 방향(CCW : Counter ClockWise direction)** 으로 정의된다.

## 후면 제거
### 후면 제거
물체 표면의 법선 벡터가 시점과 마주하면 이러한 면을 전면, 시점 방향과 나란하면(시점의 반대) 후면 이라고한다. 이 후면을 제거하는 것을 **후면 제거(Backface Culling)** 이라 한다.

물체로부터 시점으로 향하는 벡터를 시점벡터 라고 했을 떄 시점벡터 N과 법선벡터 V를 통해 Backface인지 여부를 판단할 수 있다.

$$Backface = (N \cdot V < 0) = (\vert N \vert \vert V \vert \cos\theta < 0)$$

> 정규화 가시 부피 공간에서는 시점 벡터가 z 성분만 갖고 있으므로 법선 벡터의 z값만으로 Backface를 찾을 수 있다. 따라서 후면제거는 투상 행렬을 가한 후에 이루어진다.

```c++
void glEnable(GL_CULL_FACE);	// 후면 제거 모드 활성화
void glCullFace(GLenum mode);	// GL_FRONT, GL_BACK, GL_FRONT_AND_BACK
void glDisable(GL_CULLFACE);	// 후면 제거 모드 비활성화
```

### 표면과 이면
이 함수로 표면과 이면을 선택할 수 있다.
```c++
void glFrontFace(GLenum mode); // GL_CCW, GL_CW
```

## 절단 알고리즘
점, 선, 면 등이 절단될 때 절단의 기준이 되는 것을 **절단 다각형** 이라고 부른다.

### 코헨 - 서더런드 알고리즘
1. 절단 다각형 주위로 영역(2차원이면 4비트, 3차원이면 6비트)을 만들고 코드를 부여한다. 이를 Outcode라 한다.
2. 선분의 양끝 점이 어느 영역에 있는지 순차적으로 아래 테스트를 한다.
- E1 = E2 = 0000 : 사각형 내부
- E1 & E2 != 0000 : 사각형 외부
- E1 != 0000, E2 = 0000 : 교차점에 의해 절단
- E1 & E2 = 0000 : 양 끝점이 사각형 밖에 있지만 서로 다른 경우. 절단

3. 분할한다
- 양 끝점 중 하나(E1)를 취하고 비트를 통해 어느 영역인지 판단
- 해당 영역을 판단하는 선 혹은 면에 선분이 지나는 점(E1')을 선택(절단)
- 원래의 점(E1)을 취했던 점(E1')으로 대체한다
- 반복

상대적으로 간단한 연산으로 많은 선분을 제거할 수 있다.

### 리앙 - 바스키 알고리즘
절단 사각형의 모든 변을 무한히 연장하고 교차점을 계산한다. 그리고 선분과의 교차점의 순서에 따라서 절단이 되는지 안되는지를 판단한다.

### 서더런드 - 핫지먼 알고리즘
규칙
1. 시작점과 끝점이 모두 내부라면 두 점 모두 추가
2. 내부에서 외부로 이동하면 교차점 생성
3. 외부에서 외부로 이동하면 모두 삭제
4. 외부에서 내부로 이동하면 교차점 생성

위 규칙을 통해 절단 다각형의 각 변,면을 기준으로 점을 생성, 삭제하여 절단하는 알고리즘

### 웨일러 - 애서톤 알고리즘
오목 다각형의 경우 절단되서 그려지는 다각형이 분리되는 경우엔 서더런드 - 핫지먼 알고리즘은 실패할 수 있다. 물론 다각형을 삼각형(볼록 다각형)으로 분할해서 그린다면 해결이 가능하다. 하지만 정점의 갯수가 많아진다면 시간이 오래 걸릴 것이다. 웨일러 - 애서톤 알고리즘은 이러한 예외처리 없이 그리는 것이 가능하다.

1. 반시계방향으로 한 정점에서 출발한다.
2. 외부에서 내부로 들어가는 경우 들어오는 정점(A)을 추가한다.
3. 계속 진행하다가 내부에서 외부로 나가는 경우 정점(B)을 추가하고 가장 처음 만난 들어오는 정점을 만날 때 까지 진행한다. (절단)
4. 다시 절단을 시작한 정점(B)부터 알고리즘을 시작한다.

### 내외부 판정 및 교차점
#### 정점의 내외부 판정
선분의 법선벡터를 N이라 하고 법선벡터의 시작점을 Q라고 했을 때 P의 내외부 판정은 다음과 같이 한다.
$$
(P-Q) \cdot N > 0 \rightarrow 외부    \\
(P-Q) \cdot N = 0 \rightarrow 선위 \\
(P-Q) \cdot N < 0 \rightarrow 내부
$$

$Ax + By + Cz + D = 0$ 인 평면과 점 $P(x, y, z, 1)$과의 평면거리 d는 다음과 같다
$$d = H \cdot P = (A,B,C,D) \cdot(x,y,z,1) = Ax + By + Cz + D$$
따라서 내외부 판단은 아래와 같아진다
$$
Ax + By + Cz + D > 0 \rightarrow 외부    \\
Ax + By + Cz + D = 0 \rightarrow 면위 \\
Ax + By + Cz + D < 0 \rightarrow 내부
$$

#### 선분과 절단면의 교차점
$$
p(t) = R + t(S - R) = (1-t)R + tS \\
x(t) = (1 - t)R_x + tS_x \\
y(t) = (1 - t)R_y + tS_y \\
z(t) = (1 - t)R_z + tS_z 
$$
위 와 같은 선분이 있을 때 절단면 판단은 아래와 같이 한다

$$
(p(t) - Q) \cdot N > 0 \rightarrow 외부    \\
(p(t) - Q) \cdot N  = 0 \rightarrow 면위 \\
(p(t) - Q) \cdot N  < 0 \rightarrow 내부
$$

이를 식으로 전개하면

$$
p(t) \cdot N = Q \cdot N \\ 
(R + t(S-R)) \cdot N = Q \cdot N \\
t = (Q-R) \cdot N/(S-R) \cdot N
$$

여기서 나온 t를 통해 가장 위에 식에 대입하면 절단면 위에 점을 구할 수 있다.

## 은면 제거
은면 제거는 보이지 않는 면을 제거하는 과정이다. 이 작업을 2차워 투상 이전에 해야하는 이유는 Depth(z 좌표)를 필요로 하기 때문이다.

### 페인터 알고리즘
폴리곤을 z축 기준으로 정렬한 후 순서대로 덧쓰는 작업이다. 하지만 가시성 사이클처럼 서로 맞물리는 경우로 인해 면을 분할해야 한다. 그렇게 되면 정렬하는 시간은 $O$($N\log{2}{N}$) 또는 $O$($N^2$) 이 된다.

### 지버퍼 알고리즘
화소 단위로 은면을 판단하는 알고리즘이다. 시점에서 가장 먼저 보이는 화소를 찾아내는 알고리즘인데 $O$($화소개수 \times N$) 만큼 소요되기 때문에 시간이 많이 소요된다. 이를 개선하기위해 **깊이 버퍼(Depth Buffer)** 라고 하는 Z-Buffer를 사용한다.  아래와 같은 알고리즘으로 수행된다.

```
for Each Polygon
{
	for Each Pixel
		Calculate z of Intersection
		if(new z < current z in Z-Buffer)
			update Z-Buffer with new z
			update FrameBuffer with Current Polygon
}
```

### 지엘의 지버퍼
```c++
void glutInitDisplayMode(unsigned int mode); // 사용할 모드 설정, GLUT_DEPTH | GLUT_RGBA
void glEnable(GL_DEPTH_TEST);
void glClear(GL_DEPTH_BUFFER_BIT); // 뎁스버퍼 초기화, glClearDepth(1.0); 과 같다
void glDisable(GL_DEPTH_TEST);
void glDepthFunc(GLenum func); // 뎁스테스트에 사용되는 로직 변경
void glDepthMask(GLboolean flag); // 뎁스버퍼 쓰기 금지,금지취소, GL_TRUE, GL_FALSE
```

> 후면 제거가 은면 제거보다 속도가 빠르므로 후면 제거를 먼저 수행한 후 은면 제거를 한다.
