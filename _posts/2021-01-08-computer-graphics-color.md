---
title : "[Graphics]컬러 처리"
layout: post
author: Kim heetae
tag : [Graphics, Color]
---
> OpenGL로 배우는 3차원 그래픽스 Chapter 03 내용 정리

# Color
빛은 전자기파의 일종인데 빛 에너지의 세기는 주기적으로 커지고 작아지기를 반복한다. 초당 바뀌는 횟수를 **주파수**라 하는데 대략 390nm와 720nm 사이에 분포되어있는 전자기파를 우리 눈에 보이는 가시광선이라한다.
빛이 물체 표면에 부딪히면 일부 주파수는 흡수되고 일부 주파수는 반사된다.

### 색상, 명도, 채도
 1.  색상 :  우세 주파수(반사된 빛 중 가장 큰 에너지를 지닌 주파수)의 색
 2. 채도 : 색의 선명도로 색상 에너지와 백색 에너지(적색부터 보라색까지 갖는 평균 에너지량)의 차이
 3. 명도 : 색상과 무관하게 감지된 에너지의 총량

### 컬러 매칭
컴퓨터 모니터에 사용되는 색상은 Red, Green, Blue로 구성된다. R,G,B의 함수로 색을 만드는 것을 **Color Matching**이라고 한다.
하지만 실제로 모든 자연색을 표현할 수 없어서 **CIE** 위원회에서 3개의 가상원색을 설정해 모든 자연색을 만들 수 있도록 모델링했다. 표현할 수 있는 색을 망라한 것을 **색 범주(Color Gamut)** 이라 하는데 아래 그림은 CIE색범주이다
![](/assets/resource/2021-01-08-computer-graphics-color/colormatching.png)
----
# Color Model
### RGB Color Model
삼중 자극 이론에 따라 x,y,z축을 R,G,B로 놓고 색을 표현하는 Model. **Red, Green, Blue**가 삼원색이다. 가산모델(삼원색을 더해 다른 색을 만듬)이고 일반적인 컬러모니터는 RGB Model을 사용한다.
![](/assets/resource/2021-01-08-computer-graphics-color/RGBColorModel.png)

### CMY Color Model
빛이 물체에 반사되었을 때의 색을 표현하는 Model. **Magenta, Yellow, Cyan**이 삼원색이다. 감산모델(백색광에서 다른색을 빼서 색을 만듬)이고 프린터에 사용된다.
![](/assets/resource/2021-01-08-computer-graphics-color/CMYColorModel.png)

> CMYK는 실제 잉크를 뿌릴 때 생기는 오차를 막기 위해 K로 회색농도를 넣어 표현하는 Model

### HSV ColorModel
**색상(Hue), 채도(Saturation), 명도(Value,Brightness)** 로 색을 표현하는 Model. 
![](/assets/resource/2021-01-08-computer-graphics-color/HSVColorModel.png)
----
# Color Mode
### RGB Color Mode
프레임 버퍼 내용이 **R,G,B값을 직접 담아** 표현하는 방법. 화소당 비트 수는 컬러의 정밀도와 직결된다.
![](/assets/resource/2021-01-08-computer-graphics-color/RGBColorMode.png)

### Index Color Mode
R,G,B 별로 **컬러 보기표(Color Lookup Table)** 라는 일종의 Table을 따로 정의해두고 Index를 부여한 후, 프레임 버퍼에 비트값이 해당 Index를 담아 표현하는 방법. 컬러 보기표의 값에 다양한 값을 넣을 수 있으므로 컬러의 정밀도는 올라갈 수 있지만 프레임 버퍼는 한정되어 있으므로 결론적으로 표현할 수 있는 색의 수는 같다.
![](/assets/resource/2021-01-08-computer-graphics-color/IndexColorMode.png)

