---
title: "About Unreal Engine"
layout: post
date: 2019-06-16 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- UnrealEngine
- Unreal
- Engine
- UnityVSUnreal
- Unity
category: blog
author: heetaekim
description: 유니티 사용자가 입문한 언리얼 엔진에 대해
---

# Unity와 Unreal
Unity 엔진을 사용하다가 최근부터 Unreal 엔진을 사용하게 되었다. 접하면서 헷갈렷던 부분들이 있어서 이 부분에 대해 언급해보려고 한다. 이 포스트는 Unity 사용자가 Unreal을 입문하기 위한 글이 아니고 입문하면서 필자가 개인적으로 느꼈던 헷갈린 부분들을 정리한 글이라는 것을 인지하고 읽었으면 한다.

### C# vs C++
당연히 알겠지만 Unity는 C#을 사용하고 Unreal은 C++을 사용한다. 물론 Unreal에서 C++을 사용할 때는 Unreal에서 제공되는 함수들을 사용하기 때문에 프레임워크 안에서 사용하게되어 C#과 유사한 점이 많긴 했지만 그래도 C++은 C++이기에 헷갈리는 점이 많았다.

##### struct new operation
C#에서 struct에 new Operation을 사용하면 Stack에 쌓이게 된다. 즉 가비지컬렉터를 호출하지 않게 된다. 하지만 C++에서 new Operation을 사용하면 객체를 만들어 Heap에 쌓이게 된다. 그리고 C++은 가비지컬렉터가 없기때문에 제대로 해제해주지 않으면 메모리누수가 발생한다.

##### Garbage Collector
C++에는 가비지컬렉터가 없다. 하지만 Unreal에서는 가비지컬렉터처럼 동작하는 모듈을 만들어놨다. 하지만 이 모듈을 사용하려면 어떤 객체를 생성할 때 UPROPERTY() 매크로를 사용하거나 UnrealEngine에서 제공하는 자료구조 컨테이너는 사용해 관리해서 Reference Graph에서 인지할 수 있도록 만들어줘야 한다.

##### delegate
C#에는 delegate에 대한 문법을 지원한다. 하지만 C++에서는 지원하지 않기 때문에 Unreal에서 자체적으로 만들어서 제공하고 있다. 하지만 역시 C#의 delegate, event 키워드를 사용하는 것 보다는 훨씬 불편하다. 예를 들면 파라미터 갯수마다 다른 매크로를 사용해주어야하는 등..

##### Property & LINQ
C#에서는 Property와 LINQ가 제공되는데 이는 매우 편리하다. 이 부분은 프로젝트마다 다를 수 있겠지만 처음 내가 가장 헷갈렸던 점은 기존 C# 프로젝트에서는 프로퍼티를 파스칼케이스를 사용하고 멤버변수는 카멜케이스를 사용했는데 C++에서는 프로퍼티가 없어서 파스칼케이스를 사용하는 변수명을 볼 때 마다 당황했다. 그리고 프로퍼티가 없기 때문에 Getter, Setter를 만드는데 이 부분도 어색했다. 그리고 LINQ도 없다. 사실 아직 많이 살펴보지 않아서 비슷한 문법을 Unreal에서 지원해주는지는 잘 모르겠지만 C++자체에서 제공해주는 LINQ는 없다.



### 갖춰져있는 틀
처음 접하면서 든 생각은 프로젝트규모, 구조마다 다 다르겠지만 Unreal은 이미 갖춰져있는 구조안에서 로직을 쌓아가는 방식으로 개발한다는 느낌을 받았다. 지금은 많은 기능이 생겼지만 GameMode, World, Actor, Pawn.. 과 같이 명확한 구조를 갖는 모듈들이 없기 때문에 

### Blueprint

### Prefab vs Blueprint Class

### NGUI vs UMG

### 데이터 관리
