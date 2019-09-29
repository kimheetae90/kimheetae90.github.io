---
title: "GLFW를 사용한 OpenGL Project Setting"
layout: post
date: 2019-09-29 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- OpenGL
- GLFW
- GLAD
- Graphics
category: blog
author: heetaekim
description: GLFW를 사용해서 OpenGL 프로젝트를 만들어보자
---

# OpenGL
---
OpenGL은 Open Graphics Library의 약자이고 Computer Graphics 표준 API이다. 다른 그래픽 라이브러리로는 DirectX, Metal 등이 있다. 그래픽스를 공부하기 위해서는 그래픽 라이브러리를 필수적으로 사용해야 하는데 그 중 Vulkan을 공부하고싶어서 그 조상격인 OpenGL을 다시 살펴보게 되었다. OpenGL은 GL과 GLU로 나누어져있고 GL은 기본적인 함수들, GLU는 더 쉽게 사용할 수 있도록 래핑된 함수들로 구성되어있다. 현재 GL과 GLU는 업데이트가 되지 않고 있기 때문에 GLFW, GLEW, SFML, SDL, FreeGLUT 등을 사용해야 한다. 그 중 필자는 GLFW, GLEW를 사용해보려고 한다. 이 글에서는 GLFW를 먼저 사용해보도록 하겠다.

# GLFW
---
실제로 OpenGL은 구현되어있는 라이브러리가 아니고 API 스펙이다. 스펙을 구현한 라이브러리들이 위에서 언급한 GLFW, GLEW, SFML, SDL, FreeGLUT이다. 이 중 GLFW는 멀티플랫폼 라이브러리로 OpenGL, OpenGL ES, Vulkan 개발을 위한 오픈소스이다. C로 제작되어있고 Windows, macOS 등에서 지원된다.

---
&nbsp;&nbsp;&nbsp;&nbsp;
## GLFW와 CMake 다운로드
먼저 필요한 파일들을 먼저 다운로드, 설치하도록 하자.

&nbsp;&nbsp;&nbsp;&nbsp;
## GLFW 다운로드
https://www.glfw.org/download.html 

![GLFW 다운로드](/assets/images/post/2019-09-29-OpenGL-GLFW/installGLFW.jpg)

해당 링크에 접속하여 Source package를 다운로드하고 압축을 푼다.

&nbsp;&nbsp;&nbsp;&nbsp;
## CMake 다운로드
http://www.cmake.org/cmake/resources/software.html

![Cmake 다운로드](/assets/images/post/2019-09-29-OpenGL-GLFW/installCmake.jpg)

그 다음은 이미 CMake가 설치되어있다면 스킵해도 좋다. CMake를 사용한 적이 없다면 해당 링크에 접속하여 CMake를 다운로드한 후 설치한다.


&nbsp;&nbsp;&nbsp;&nbsp;
## GLFW 소스 컴파일
위에 두 개를 다운로드하고 설치했다면 다운로드한 GLFW를 컴파일해서 lib를 생성하면 된다. 

![build 폴더 생성](/assets/images/post/2019-09-29-OpenGL-GLFW/makeBuildFolder.jpg)

컴파일하기 전에 GLFW 압축파일을 풀어서 생성된 폴더 안에 build라는 폴더를 하나 생성한다(꼭 build일 필요는 없지만 알아보기 쉽게 하기 위해)

![경로설정](/assets/images/post/2019-09-29-OpenGL-GLFW/cmakepath.jpg)

그 후 CMake를 실행해서 빌드될 source와 생성된 binary의 path를 설정해준다. source path는 압축해제한 GLFW폴더이고 생성된 binary를 둘 path는 새로 생성한 build 폴더이다

![GLFW 프로젝트 생성](/assets/images/post/2019-09-29-OpenGL-GLFW/cmakeConfig.jpg)

설정이 완료되면 Configure 버튼을 누르는데 누르고 나면 라이브러리를 구성하기 위한 옵션을 보여줄 것이다. 그 때 그냥 기본값을 두고 Configure 버튼을 한번 더 누르면 설정을 저장할 수 있다.(프로젝트 생성기를 생성할때는 자신이 사용할 IDE에 맞게 선택한다)

![GLFW 프로젝트 생성완료](/assets/images/post/2019-09-29-OpenGL-GLFW/glfwprojectinit.jpg)

저장한 후 Generate 버튼을 눌러서 프로젝트 파일을 생성한다. 그럼 위와 같이 build 폴더에 프로젝트가 생성된다! 

![라이브러리파일](/assets/images/post/2019-09-29-OpenGL-GLFW/glfw3lib.jpg)

솔루션을 열어서 솔루션을 빌드한다. 그러면 src/Debug 폴더에 glfw3.lib라는 파일이 생성된다



&nbsp;&nbsp;&nbsp;&nbsp;
## 프로젝트 생성

이제 프로젝트를 생성한다. 우리는 위에서 생성한 라이브러리파일을 사용할 것인데 시스템에 위치시키던 프로젝트안에 위치시키던 본인이 편한 방식으로 세팅한다. 이 글에서는 프로젝트 안에 세팅할 것이다.

![폴더생성1](/assets/images/post/2019-09-29-OpenGL-GLFW/libraryfolder.jpg)

![폴더생성2](/assets/images/post/2019-09-29-OpenGL-GLFW/libraryfolder2.jpg)

프로젝트 안에 라이브러리를 둘 폴더를 생성하고 그안에 include와 library를 나눠서 배치할 수 있도록 폴더를 추가생성한다.

![폴더생성2](/assets/images/post/2019-09-29-OpenGL-GLFW/glfwinclude.jpg)

위에서 생성된 lib를 lib를 배치하기 위해 만든 폴더에 배치하고 GLFW 내에 include 폴더 안에 있는 헤더들은 include 파일을 넣기 위한 폴더에 배치한다.

&nbsp;&nbsp;&nbsp;&nbsp;
## Linking

![라이브러리 경로 설정](/assets/images/post/2019-09-29-OpenGL-GLFW/libpath.jpg)

프로젝트가 GLFW를 사용할 수 있도록 include와 library의 디렉토리를 알려주도록 한다.

![라이브러리 경로 설정](/assets/images/post/2019-09-29-OpenGL-GLFW/liblinker.jpg)

그리고 Linker에서 프로젝트와 lib를 연결해준다. 이 때 opengl32.lib도 연결해준다.


&nbsp;&nbsp;&nbsp;&nbsp;

# GLAD
---
OpenGL 드라이버의 버전은 많기 때문에 대부분의 함수 위치는 컴파일을 할 때 알 수 없으므로 런타임에서 체크해야한다. 이 위치를 개발자가 포인터로 저장해두어야 하는데 이 위치는 운영체제마다 다르다. 그렇기 때문에 알맞는 함수들을 맵핑해주는 GLAD라는 오픈소스 라이브러리를 사용할 것이다.

&nbsp;&nbsp;&nbsp;&nbsp;
## GLAD 다운로드

https://glad.dav1d.de/

![GLAD 다운로드 세팅1](/assets/images/post/2019-09-29-OpenGL-GLFW/glad.jpg)

![GLAD 다운로드 세팅2](/assets/images/post/2019-09-29-OpenGL-GLFW/glad2.jpg)

위 경로로 접속하면 웹서비스가 나온다. 이 페이지에서 C++, 3.3이상의 OpenGL버전, 프로파일을 Core로 설정한 후 Generate a loader를 선택한 후에 Generate 버튼을 클릭한다.

그리고 파일을 다운로드한 후에 include폴더들(glad,KHR)은 glfw의 헤더를 넣은 곳과 같은 위치, glad.c는 원하는 위치에 놓고 프로젝트 안에 포함시켜준다.

&nbsp;&nbsp;&nbsp;&nbsp;
### 코드
이제 준비는 완료되었다. 아래 코드를 기입한 후 컴파일 한다.

```
#include <iostream>
#include <glad/glad.h>
#include <glfw3.h>

void framebuffer_size_callback(GLFWwindow* window, int width, int height)
{
	glViewport(0, 0, width, height);
}

void processInput(GLFWwindow *window)
{
	if (glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)
		glfwSetWindowShouldClose(window, true);
}

int main()
{
	glfwInit();
	glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
	glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
	glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

	GLFWwindow* window = glfwCreateWindow(800, 600, "Window", NULL, NULL);
	if (window == NULL)
	{
		std::cout << "Failed to create GLFW window" << std::endl;
		glfwTerminate();
		return -1;
	}
	glfwMakeContextCurrent(window);

	
	if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
	{
		std::cout << "Failed to initialize GLAD" << std::endl;
		return -1;
	}

	glViewport(0, 0, 800, 600);
	glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);

	while (!glfwWindowShouldClose(window))
	{
		processInput(window);

		glClearColor(0.6f, 0.2f, 0.3f, 1.0f);
		glClear(GL_COLOR_BUFFER_BIT);

		glfwPollEvents();
		glfwSwapBuffers(window);
	}

	glfwTerminate();

	return 0;
}
```

### 간략 설명

- glfwInit : GLFW 초기화

- glfwWindowHint : GLFW 설정

- GLFW_CONTEXT_VERSION_MAJOR, GLFW_CONTEXT_VERSION_MINOR : 사용할 OpenGL의 메이저버전과 마이너버전 설정

- GLFW_OPENGL_CORE_PROFILE : OpenGL의 작은 집합에 접근하기 위한 설정

    cf) MacOSX에서는 glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE); 를 추가로 넣어야함

- glfwCreateWindow : 윈도우 생성

- gladLoadGLLoader : GLAD가 올바른 함수를 로드할 수 있도록 초기화

- glViewport : 윈도우 좌표 정의

- glfwSetFramebufferSizeCallback : 창 크기 변경시의 콜백 등록

- glfwWindowShouldClose : GLFW가 종료되었는지 확인

- glfwSwapBuffers : DoubleBuffering

- glfwPollEvents : 이벤트 발생 여부를 확인하고 윈도우 업데이트할 콜백함수 호출

- glfwTerminate : 모든 자원 정리 및 삭제

- glfwGetKey : key 입력받기

- glClearColor, glClear : 화면 지우기


참조 : https://learnopengl.com/Getting-started/Hello-Window


