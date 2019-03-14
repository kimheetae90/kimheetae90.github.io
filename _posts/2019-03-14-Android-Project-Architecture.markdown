---
title: "[Android] Project 구조"
layout: post
date: 2019-03-14 19:00
image: /assets/images/markdown.jpg
headerImage: false
tag:
- android
category: blog
author: heetaekim
description: Android Project 구조를 이해하자!
---
# Module
---
소스 파일 및 빌드 설정으로 구성된 모음이다. 프로젝트는 하나 이상의 모듈로 구성되고 모듈은 다른 모듈에 종속성으로 사용할 수 있다. 
* Android App Module

 앱 소스 코드, 리소스 파일, 앱 수준 등을 포함할 수 있는 컨테이너 제공. 프로젝트를 새로 만들면 기본 모듈 이름인 'app'으로 생성된다.

* Library Module

 Android Project에서 앱을 빌드하는데 필요한 소스코드, 리소스파일, 매니페스트 등을 포함할 수 있는 컨테이너인 Android Library와 Java 소스파일인 Java Library. Android Library는 빌드 시 AAR(Android Archive)로 생성되고 Java Library는 JAR(Java Archive)로 생성됨

* Google Cloud Module

 Google Cloud Backend 코드를 포함할 수 있는 컨테이너 제공

&nbsp;
&nbsp;
# Android Project
---
### Android View로 보는 Project 구조
Android Studio는 기본적으로 Android View로 프로젝트 파일을 표시한다. 사용되지 않는 파일은 숨기고 프로젝트에서 필요한 파일들을 정리해서 보여준다. 여기서 보여지는 구조는 실제 디스크에 있는 파일구조와는 다르다.

![Android View](/assets/images/post/2019-03-12-Dictionary-Key-Garbage/2019-03-12-result-garbage.jpg)

##### app Module
프로젝트 생성시 기본으로 생기는 Android App Module.
* manifests : AndroidManifest.xml 포함한다. (AndroidManifest는 아래에서 설명)
* java : Java 소스코드(클래스)를 관리한다. 패키지 이름별로 구분된다.
* res : Resource 폴더로 layout, ui, 문자열 등 모든 리소스를 포함한다.

##### Gradle Scripts
어플리케이션을 빌드하기위해 필요한 설정 옵션, 라이브러리 정보들 포함. Gradle은 ant, maven과 같은 빌드 배포 도구로 라이브러리를 관리해준다. Android Studio에서는 Gradle을 채택하여 사용하고 있다. 

&nbsp;
### Project View로 보는 Project 구조
Android View에서 숨겨진 모든 파일을 포함하여 프로젝트의 실제 파일구조를 보여준다.

![Project View](/assets/images/post/2019-03-12-Dictionary-Key-Garbage/2019-03-12-result-garbage.jpg)

아래 목록은 주요 모듈에 대한 설명이다.
```
build/ : 빌드 출력을 포함
libs/ : 프로젝트에 추가되는 외부 라이브러리 포함
src/ : 모든 코드 및 리소스 포함. 처음 실행될 때 MainActivity 클래스를 이용한다.
AndroidMenifest.xml : AndroidManifest는 안드로이드 어플리케이션을 구동하는데 필요한 설정값, 권한 정보 등을 관리하는 파일
java/ : Java 소스 코드 포함
jni/ : JNI(Java Native Interface)를 사용하는 네이티브 코드 포함
gen/ : Android Studio에서 생성하는 Java파일 포함(예 : R.java)
R.java : 프로젝트의 객체에 접근할 수 있는 ID파일. 임의로 편집하면 안됨
res/ : 리소스 파일 포함
drawable : 비트맵 형식의 파일과 이미지를 나타내는 xml 포함. 해상도에 따라 hdpi, mdpi, ldpi로 나뉨
layout : App을 디자인한 xml 레이아웃 포함
value : 프로젝트에서 사용하는 값을 정의한 xml(문자열에 대한 정의 등) 포함
assets/ : 그대로 컴파일되어야 하는 파일 포함. 텍스처 및 게임데이터에 적합.
default.properties : 프로젝트 설정과 관련한 속성을 정의한 파일.(예 : 빌드타겟) 이 파일이 없으면 java 코드에 하드코딩 해야한다.

```



