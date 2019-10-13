---
title: "OpenGL Rendering Pipeline"
layout: post
date: 2019-10-13 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- OpenGL
- RenderingPipeline
- Graphics
category: blog
author: heetaekim
description: OpenGL Rendering Pipeline에 대해 알아보자!
---

# Rendering Pipeline
어떠한 데이터를 화면에 그리는 것을 Rendering이라고 한다. Graphics에서는 2D 혹은 3D 데이터를 화면에 그리게 되는데 이 때 일련의 과정을 거치게 된다. 이 것을 Rendering Pipeline이라고 한다.

# OpenGL Rendering Pipeline
각 Graphics Library마다 Rendering Pipeline이 있다. 대부분 유사하지만 약간의 차이가 있다. 필자는 이번에 OpenGL의 Rendering Pipeline에 대해서 알아보았다.

![Rendering Pipeline](/assets/images/post/2019-10-13-OpenGL-Rendering-Pipeline/pipeline.jpg)


# Vertex Specification
-----
이 단계에서는 정렬된 vertex list를 pipeline으로 보내는 일을 한다. pipeline으로 보낸다는 것은 GPU로 전송하는 것을 의미한다. 데이터를 보낼 때 vertex들은 primitive라는 기본 개념으로 정의된 형태로 전달이 된다. primitive는 삼각형, 선, 점 등의 기본 모양이다.
 GPU로 데이터를 전송 할 때 매 프레임마다 매번 데이터를 보내는 것은 아주 비효율적인 일이다. 그렇기때문에 Buffer Object라는 개념을 통해 최대한 많은 양을 보내고 GPU 메모리에 저장한 후 사용하여 최소한의 횟수로 보내게 된다.

&nbsp;&nbsp;&nbsp;&nbsp;
## Buffer Object
Buffer Object에는 VBO, VAO가 있는데 기존에 lagacy code에 있던 glBegin과 glEnd가 제거되면서 생겨났다. VBO에 값을 만들어준 후 VAO에 맵핑해주고 그 정보로 그림을 그리는 것이다.

&nbsp;&nbsp;&nbsp;&nbsp;
### VBO
Vertext Buffer Object의 약자이다. vertex data를 직접적으로 가지고 있다. vertex data는 vertex attribute를 표현할 때 사용되며 vertex attribute로는 position, normal, color 등이 있다. 아래와 같이 생성할 수 있다.

```
unsigned int buffer;
glGenBuffers(1,&buffer);
glBindBuffer(GL_ARRAY_BUFFER,buffer);
glBufferData(GL_ARRAY_BUFFER,sizeof(vertices),vertices,GL_STATIC_DRAW);
```

- glGenBuffer : buffer id 생성
- glBindBuffer : GL_ARRAY_BUFFER에 2번째 id로 바인딩, 0을 파라미터로 넣으면 Unbind된다.
- glBufferData : 두번째 파라미터의 크기만큼 세번째 파라미터에 해당하는 내용을 저장. 마지막 파라미터는 데이터를 관리하는 방법인데 GL_STATIC_DRAW, GL_DYNAMIC_DRAW, GL_STREAM_DRAW등이 있다.

&nbsp;&nbsp;&nbsp;&nbsp;
### VAO
Vertex Array Object의 약자로 vertex buffer와 index buffer의 레퍼런스를 저장하고 있는 데이터이다. VAO는 총 16개의 VBO 레퍼런스를 담을 수 있고(하드웨어에 따라 다르다고 함) 그에 따라 vertex attribute를 입힐 수 있다. 과거에는 VAO는 1개만 존재했지만 지금은 여러개를 선언하여 사용할 수 있게 되었다. VAO를 생성하는 방법은 아래와 같다.
```
unsigned int VAO;
glGenVertexArrays(1, &VAO);  
```
- glGenVertexArrays : vao id 생성

VAO를 사용하기 위해서는 VBO와 바인딩해줘야 한다. VAO가 바인딩되어있지 않다면 그림을 그릴 수 없다. VBO와 바인딩하는 법은 아래와 같다.
```
glBindVertexArray(VAO);
glBindBuffer(GL_ARRAY_BUFFER, VBO);
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);  
```
- glBindVertexArray : 사용할 VAO를 바인딩함
- glVertexAttribPointer : OpenGL이 vertex 데이터를 어떤 attribute를 표현할 때 사용할 지 알려줌, 첫번째 파라미터로 vertex shader에서 사용할 속성의 위치를 넘기고 두번째 파라미터는 속성의 크기를 전달한다. 세번째는 넘겨질 데이터의 타입이고 네번째는 데이터를 정규화 할 것인지, 즉 -1과 1 사이의 값으로 맞춰줄 것인지에 대한 플래그이다. 다섯번째는 stride라는 값인데 데이터는 공백이 없는 연결된 데이터인데 다음 속성까지 몇칸의 데이터를 넘겨야할지에 대한 값을 넘긴다. 마지막 파라미터는 속성이 시작될 데이터의 offset이다.

EBO라는 인덱스 버퍼도 있지만 이 포스트에서는 다루지 않겠다. VAO와 VBO의 관계를 그림으로 나타내면 다음과 같다

![VAO와 VBO](/assets/images/post/2019-10-13-OpenGL-Rendering-Pipeline/vaonvbo.jpg)
출처 : https://learnopengl.com/Getting-started/Hello-Triangle›

위와 같이 전달받은 vertex data들은 다음 과정인 Vertex Processing을 통해 가공된다.

&nbsp;&nbsp;&nbsp;&nbsp;

## Shader
Shader는 GPU에서 실행되는 프로그램으로 OpenGL에서는 GLSL(OpenGL Shading Language)라는 언어가 사용된다. glsl shader 코드는 다음과 같은 형태로 표현된다
```
#version version_number
in type in_variable_name;
in type in_variable_name;

out type out_variable_name;
  
uniform type uniform_name;
  
void main()
{
  ...
  out_variable_name = weird_stuff_we_processed;
}
```

&nbsp;&nbsp;&nbsp;&nbsp;
# Vertex Processing
-----
Vertex Processing은 사용자가 제어할 수 있는 프로세스이다. vertex shader, tessellation shader, geometry shader를 통해서 제어가 가능하다.

## Vertex Shader
Vertex Shader에서는 각 정점의 기본 처리를 수행한다. vertex shader에서 vertex의 각 attribute를 입력받아 제어한 후 최종적으로는 미리 정의되어있는 gl_Position이라는 값에 제어한 값을 입력한다. 각 vertex를 제어해야하므로 vertex의 갯수만큼 호출된다. 이 단계는 필수로 존재해야하는 단계이다.

## Tessellation
Tessellation이란 고차의 Primitive를 더 작고 단순하며 렌더링 가능한 Primitive로 쪼개는 과정이다. Tessellation은 Tessellation Control Shader, Tessellation Engine, Tessellation Evaluation Shader 순서로 제어된다. 이 단계는 필수로 작성하지 않아도 된다.

## Geometry Shader
geometry shader는 입력되는 primitive를 제어해서 다른 형태의 primitive로 반환해주는 shader이다. 따라서 vertex의 세트를 입력은 받게되고 주어진 것 이상의 vertex로 가공하여 내보낼 수 있다. 이 단계도 마찬가지로 필수가 아니다

&nbsp;&nbsp;&nbsp;&nbsp;
# Vertex Post Processing
이 단계는 고정 기능 단계로 사용자의 제어 권한이 제한되는 단계이다. Vertex Processing에서 가공된 최종 결과는 일련의 버퍼 객체에 기록된다. 이를 Transform feedback mode라고 한다. 또 Clipping이라는 과정을 거치는데 volume밖에 있는 primitive들을 버리는 일을 한다.

&nbsp;&nbsp;&nbsp;&nbsp;
# Primitive Assembly
Primitive Assembly의 단게에서는 이전 단계에서 출력된 vertex data를 수집하여 primitive sequence로 구성하는 과정이다. 이 것들은 다음 단계인 Rasterization의 가이드라인이 된다. 사용자가 어떤 방식으로 렌더링 형식을 정했느냐에 따라 작동 방식이 결정된다. 만약 tessellation이나 GS가 활성화되어있으면 이 단계는 vertex processing 전에 제한적으로 수행된다. 이 단계에서는 Face Culling도 하게 되는데 Face Culling이란 window space에서 그려지지 않아도 되는 primitive들을 그리지 않게 제거하는 과정이다.

&nbsp;&nbsp;&nbsp;&nbsp;
# Rasterization
이 단계에 오게 된 primitive는 순서대로 Rasterization 된다. 이것의 결과물은 fragment의 sequence이다. fragment는 pixel의 최종 데이터를 그리는데 사용되는 상태 집합이다. 예를 들면 색상, 깊이 등이 된다. 이 때 fragment는 vertex data의 사이값으로 정해지게 된다.

&nbsp;&nbsp;&nbsp;&nbsp;
# Fragment Processing
Rasterization의 결과로 나온 fragment들은 fragment shader에 의해 다시 제어된다. fragment shader의 출력값은 색상, 깊이, 스텐실 값 등이다. 이 단계는 필수로 작성해야하는 단계가 아닌데 fragment shader를 작성하지 않고 렌더링하게 되면 기본값으로 출력하게 된다. 이 단계는 보통 전체 렌더링 시간의 96%정도 소요하게 된다. 왜냐하면 fragment shader는 각 fragment마다 실행되기 때문이다. 예를 들어 vertex가 3개인 삼각형이 있을 때 그 내부에서 보간되어 생긴 fragment가 100개라고 쳤을 때 vs는 vertex의 갯수인 3만큼 동작하지만 fragment는 100만큼 동작하게 된다. 이 단계를 거치면 pixel의 최종 색상이 결정된다.

&nbsp;&nbsp;&nbsp;&nbsp;
# Per Sample Operations
마지막으로 생성된 fragment data들은 몇가지 단계의 테스트를 거친 후 통과가 되어야만 출력이 된다. 이 단계는 사용자가 활성화 한 경우에 작동하게 되고 최적화를 위해 이 단계를 앞으로 빼는 경우도 있다.

### pixel ownership test
다른 window가 존재하는 경우 gl 윈도우가 pixel에 대한 소유권을 갖지 못했을 때 실패한다.

### Scissor Test
화면에 지정된 사각형 외부에 있으면 실패한다.

### Stencil Test
Stencil buffer라는 buffer기반으로 수행되는데 사ㅛㅇ자가 특정 stencil값을 설정하여 fragment를 폐기할지 유지할지 결정한다.

### Depth Test
Depth buffer 기반으로 수행되는데 특정 fragment가 앞인지 뒤인지를 판별하여 폐기할지 유지할지를 결정한다.

모든 테스트를 통과하면 color blending을 실행하여 최종 데이터를 만들어낸 후 frame buffer에 저장하고 이 값을 출력하게 된다.
