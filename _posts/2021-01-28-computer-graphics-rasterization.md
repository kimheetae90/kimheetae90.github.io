---
title : "[Graphics]래스터 변환"
layout: post
author: Kim heetae
mathjax: true
tag : [Graphics, Transfrom]
---
> OpenGL로 배우는 3차원 그래픽스 Chapter 09 내용 정리

## 래스터 변환
래스터(Raster)는 화소(Pixel)을 의미한다. 즉, **래스터 변환(Rasterization)** 은 화소로의 사상을 말한다.

![](/assets/resource/2021-01-16-computer-graphics-visibility/rasterizationinpipeline.PNG)

은면 제거 과정에서 색과 깊이가 모두 사용되므로 뷰 포트 변환 결과의 정점을 대상으로 먼저 가해진다.

## 선분의 래스터 변환
래스터 변환은 선분을 이용한다. 선분의 기울기가 1보다 작으면 x좌표를 증가시키며 화소를 칠하고 1보다 크면 y좌표를 증가시키며 화소를 칠한다.

### 교차점 계산
```c++
int dx = x2 - x1;
int dy = y2 - y1;
float m = dy / dx;
for(x = x1 ; x <= x2 ; x++)
{
	y = m*(x - x1) + y1;
	DrawPixel(x, round(y));
}
```
위와 같은 방법으로 화소를 칠하는데 이 방법은 간단하지만 느리다.

### DDA 알고리즘
```c++
int dx = x2 - x1;
int dy = y2 - y1;
float m = dy / dx;
float y = y1;
for(x = x1 ; x <= x2 ; x++)
{
	DrawPixel(x, round(y));
	y += m;
}
```
x는 1씩 증가하는데 y는 기울기 값 만큼 증가하는 것을 이용한 방법이다. 하지만 이 알고리즘도 문제가있다.

1. 부동 소수 연산
2. 반올림 연산 
3. 연산 결과의 정확도

### 브레스넘 알고리즘
```c++
int dx = x2 - x1;
int dy = y2 - y1;
int D = 2 * dy - dx;
int incrE = 2 * dy;
int incrNE = 2 * dy - 2 * dx;

int x = x1;
int y = y1;
DrawPixel(x,y);
while(x < x2)
{
	if(D <= 0)
	{
		D += incrNE;
		x++;
	}
	else
	{
		D += incrNE;
		x++;, y++;
	}
	DrawPixel(x,y);
}
```
중점 알고리즘 이라고도 부른다.  x번째 화소의 다음화소를 결정할 때, 바로 옆 화소를 선택하느냐, 남,북 쪽으로 이동해서 선택하느냐로 정수 연산화 한 알고리즘으로 속도가 빠르다.

$$
y = \frac{dy}{dx}x + b\\
ydx = xdy + bdx\\
f(x,y) = ydx - xdy - bdx = 0 \\
F(x,y) = 2ydx -2xdy -2bdx = 0
$$
위 식을 기반으로 아래 식을 유도할 수 있다.
 
 $$
 F(x,y) \\
 = F(x_1 + 1, y_1 + 1/2) \\
 = 2(y_1 + 1/2)dx - 2(x_1 + 1)dy - 2bdx\\
 =2y_1dx + dx - 2x_1dy - 2dy - 2dbdx\\
F(x_1, y_1) = 2y_1dx - 2x_1dy - 2bdx = 0 \\
F(x,y) = F(x_1 + 1, y_1 + 1/2) = dx -2dy
 $$
이를 결정 변수 D 라고 한다.

## 다각형의 래스터 변환

### 삼각형의 래스터 변환

![](/assets/resource/2021-01-16-computer-graphics-visibility/triangleinner.PNG)

* P,Q,R 등 다각형의 모든 정점을 반시계로 정의
* 선분 $f(x,y) = (y_1 - y_2)x + (x_2 - x_1)y + x_1y_2 - x_2y_1 = 0$를 구할 때 먼저 정의된 정점이 $(x_1, y_1)$ , 나중에 정의된 정점이 $(x_2, y_2)$ 로 대입
* 모든 선분은 반시계로 진행되고 진행 방향의 왼쪽은 $f(x,y) > 0$, 오른쪽은 $f(x,y)<0$이 성립

위 규칙을 통해 아래 알고리즘으로 색칠한다.

```c++
Calc f1(x,y) for Edge 1;
Calc f2(x,y) for Edge 2;
Calc f3(x,y) for Edge 3;

for Each Pixel P at (a,b)
	if(f1(a,b) > 0) && f2(a,b) > 0) && f3(a,b) > 0)
		DrawPixel(a,b,Color);
```

### 주사선 채움 알고리즘

![](/assets/resource/2021-01-16-computer-graphics-visibility/linefilling.PNG)

다각형의 선분들을 리스트화(선분 리스트:EL) 하고 주사선을 아래에서부터 그린다고 했을 때, 주사선과 만나는 점을 오름차순으로 정렬한다. 그리고 홀수규칙에 의해 칠해진다. 즉, $1 \leq x < 2$, $3 \leq x < 4$ 만 칠하는 방식이다.
예외로 극대점은 0번 교차 처리, 극소점은 2번 교차 처리, 극대이자 극소점은 한번 교차 처리, 평행한 선분은 없는 처리를 한다.

```c++
Build EL;
Initialize AEL to Empty List;
Set y to the smallest y in the EL;
Repeat{
	Delete Edges from AEL;
	Move New Edges from EL into AEL
	Sort AEL based on x;
	Fill Pixels between Edge Pairs;
	Increment y;
	Update Edge x;
}until (AEL == NULL) && (EL == NULL)
```


### 씨앗 채움 알고리즘
* 경계 채움 알고리즘 : 화소 하나를 정하고 경계선이 되는 화소까지 칠하는 방법이다.
```c++
void BoundaryFill(int x, int y, color fillc, color boundc)
{
	color currc = ReadPixel(x,y);
	if(currc != boundc && currc != fillc)
	{
		SetPixel(x, y, fillc);
		BoundaryFill(x+1, y, fillc, boundc);
		BoundaryFill(x-1, y, fillc, boundc);
		BoundaryFill(x, y-1, fillc, boundc);
		BoundaryFill(x, y+1, fillc, boundc);
	}
}
```

* 홍수 채움 알고리즘 : 다각형 내부 색이 동일한 것을 전제로 현재 색이 지속될 때 까지 칠하는 방법이다.
```c++
void FloodFill(int x, int y, color fillc, color oldc)
{
	if(ReadPixel(x,y) == oldc)
	{
		FloodFill(x+1, y, fillc, oldc);
		FloodFill(x-1, y, fillc, oldc);
		FloodFill(x, y-1, fillc, oldc);
		FloodFill(x, y+1, fillc, oldc);
	}
}
```


## 보간법
### 무게중심 좌표
$$
V(t) = P + t(Q - P) = (1-t)P + tQ = \alpha P + \beta Q\\
0 \leq \alpha , \beta \leq 1, \alpha + \beta = 1
$$
선분을 위 식과 같이 표현할 때, 가중치에 해당하는 $(\alpha, \beta)$ 를 무게중심 좌표라고 표현한다.

$$
V'\\
= V + s(R - V)\\
= P + t(Q - P) + s(R - P + t(Q - P)))\\
= (1 - t - s + st)P + (t - st)Q + sR\\
= \alpha P + \beta Q + \gamma R\\
0 \leq \alpha, \beta , \gamma \leq 1 , \alpha + \beta + \gamma = 1
$$
3점을 혼합하면 삼각형을 표현할 수 있다.

> $$ V = t_1P_1 + t_2P_2 + \dotsb + t_nP_n\\ 0 \leq t_1, t_2, \dotsb,
> t_n \leq 1, t_1 + t_2 + \dotsb + t_n =1 $$ 일반화하면 n개의 점으로 표현할 수 있다
> 
> 위 식을 만족하는 도형을 **컨벡스 헐(Convex Hull)** 이라고 하고 이는 어떤 점이든 항상 컨벡스 헐 내부에
> 존재한다는 특성이 있다.

정점을 PQR로 갖는 삼각형의 내부점 V'의 무게중심좌표는 아래와 같이 계산한다.
$$
\alpha = area(V'QR) / area(PQR) \\
\beta = area(V'RP) / area(PQR) \\
\gamma = area(V'PQ) / area(PQR) 
$$

삼각형의 넓이는 외적을 통해 구할 수 있으므로 아래와 같이 치환된다

$$
\alpha = abs((V' - Q) \times (R-Q) ) / abs((P - Q) \times (R - Q)) \\
\beta = abs((V' - R) \times (P-R) ) / abs((P - Q) \times (R - Q)) \\
\gamma = abs((V' - P) \times (Q-P) ) / abs((P - Q) \times (R - Q)) \\
$$

이 식을 통해 $\alpha, \beta, \gamma$가 정해졌다면 내부 화소별 색은 다음과 같이 구할 수 있다.

$$
r = \alpha P_r + \beta Q_r + \gamma R_r\\
g = \alpha P_g + \beta Q_g + \gamma R_g\\
b = \alpha P_b + \beta Q_b + \gamma R_b\\
$$

> 깊이 버퍼도 위와 같은 공식으로 계산된다.

### 양방향 선형 보간

![](/assets/resource/2021-01-16-computer-graphics-visibility/bilinearinterpolation.PNG)

위 그림에서 $S, T, V$ 를 순차적으로 아래와 같은 식을 통해 구한다.

$$
S = \frac{a1}{a1+a2}P + \frac{a2}{a1+a2}R \\
T = \frac{b1}{b1+b2}Q + \frac{b2}{b1+b2}R\\
V = \frac{c1}{c1+c2}T + \frac{c2}{c1+c2}S
$$

그럼 최종 결론은 아래와 같이 나온다.

$$
V = \frac{c2}{c1+c2}S + \frac{c1}{c1+c2}T\\
= \frac{c2}{c1+c2}(\frac{a1}{a1+a2}P + \frac{a2}{a1+a2}R) + \frac{c1}{c1+c2}(\frac{b1}{b1+b2}Q + \frac{b2}{b1+b2}R) \\
= \alpha P + \beta Q + \gamma R
$$


## 지엘의 그래픽 기본 요소
### 기본 요소 정의
점(GL_POINTS), 선(GL_LINES), 도형(GL_POLYGON) 등으로 구성된다.
GL_TRIANGLES, GL_QUADS, GL_POLYGON은 다각형을 그리기 위한 요소이다. 이를 사용하기 위해서는 아래 조건을 만족해야 한다.
* 단순 다각형
* 볼록 다각형
* 평면 다각형

> 삼각형 이상의 도형을 그릴 때 지엘은 분할(Tessellation) 하지 않으므로 프로그래머가 유의해야한다.

지엘에서 기본 요소를 그리기 전에는 아래 함수로 속성을 명시해야 한다.
```c++
void glPointSize(GLfloat size); //정점 크기, 기본 1.0
void glLineWidth(GLfloat width); // 선분의 두께, 기본 1.0
void glLineStipple(GLint factor, GLushort pattern); // 점선, 쇄선을 그림, 0은 그리지 않음
glEnable(GL_LINE_SIPPLE); , glDisable(GL_LINE_SIPPLE);
void glPolygonStripple(const GLubyte *mask); // 다각형 내부 패턴
void glShadeModel(mode); // 다각형 채움 모드
```


