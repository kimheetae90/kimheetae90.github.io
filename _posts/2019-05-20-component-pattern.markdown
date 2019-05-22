---
title: "Component Pattern"
layout: post
date: 2019-05-20 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Design Pattern
- Component Pattern
- Unity
category: blog
author: heetaekim
description: Component Pattern의 소개와 사용법 및 실례
---

# Component Pattern
---
Component Pattern은 한 개체가 여러 분야를 서로 커플링없이 다룰 수 있게 하는 패턴이다. 시스템 하나를 통 째로 일체형으로 구축하지 않고 기능이나 요소들을 쪼개서 Component로 부품화하여 만든 후 조립하여 사용하는 방식이다. 그리고 개체는 컴포넌트들의 컨테이너 역할만 하는 껍데기가 된다. 예를 들자면 컴퓨터를 조립할 때 케이스, 메인보드, cpu, 메모리 등등 여러 부품을 조립해서 만들 수 있을 것이다. 그리고 각 부품은 다른 부품으로 교체 할 수 있고 원하면 추가할 수 도 있고 포트를 활용해 다른 기능을 하는 기기를 연결할 수 도 있다. 이러한 개념을 인지하고 이번 포스팅을 읽어보자

&nbsp;&nbsp;&nbsp;&nbsp;


# 사용하기 적절한 때?
---

### 문제점
컴퓨터을 만들 때 컴퓨터는 여러가지 일을 할 것이다. 이를 코드로 간단히 구현해보도록 하자! 컴퓨터는 출력장치, cpu, 메모리, 입력장치 등 여러가지 기능을 하는 부품들을 모아서 만들 수 있을 것이다. 하지만 이 모든 기능을 한 객체에 전부 담게 되면 코드가 길어지게 되고 각 기능들이 서로 커플링되게 될 것이다.

 그리고 컴퓨터는 PC, 노트북, 태블릿, 스마트폰 등 여러가지 모델이 있을 것이고 이를 상속으로 해결하려한다면 설계가 어려울 뿐더러 단일 상속으로는 다른 클래스에서 중복되는 기능이 생길 수 있고 다중 상속을 한다면 다이아몬드 구조가 생길 것이다.

&nbsp;&nbsp;&nbsp;&nbsp;

### 해결법
이를 해결하기 위해 상속으로 해결하기 보다 각 기능을 부품화하여 나누면 기능 별로 코드를 나눌 수 있기 때문에 한 개체에 들어가는 코드량도 적어지고 각 기능들을 디커플링 할 수 있다. 또 기능만을 따로 분리했기 때문에 다른 개체에서 사용할 수 도 있어 재사용성이 높아진다.

&nbsp;&nbsp;&nbsp;&nbsp;


# 코드로 보는 예시
---
먼저 위 예시를 개체 하나에 다 담은 코드이다.

##### ComputerFull
```
class ComputerFull
{
public :
	void ComputeAndPrint()
	{
		cin >> number1;
		cin >> operation;
		cin >> number2;

		int result = 0;

		switch (operation)
		{
		case '+':
			result = number1 + number2;
			break;
		case '-':
			result = number1 - number2;
			break;
		case '*':
			result = number1 * number2;
			break;
		case '/':
			result = number1 / number2;
			break;
		default:
			break;
		}


		cout << number1 << operation << number2 << " = " << result;
	}

private :
	int number1;
	int number2;
	char operation;
};
```

이 코드는 모든 기능을 포함하고 있는 통짜 클래스이다. 코드의 길이가 길고 각 기능이 번잡하게 엮여있다. 이 코드를 각 기능별로 분할해보겠다

##### Memory
```
class Memory
{
public :
	int number1;
	int number2;
	char operation;
	int result;
};
```
통짜 클래스에서 private에서 변수를 저장하고 있는 부분을 Memory라는 Component로 분류했다.

##### Keyboard
```
class Keyboard
{
public:
	void Input(Memory& memory)
	{
		cin >> memory.number1;
		cin >> memory.operation;
		cin >> memory.number2;
	}
};
```
외부로부터 입력을 받아서 변수에 저장하던 부분을 Keyboard라는 Component로 분류했다.

##### CPU
```
class CPU
{
public:
	void Compute(Memory& memory)
	{
		int number1 = memory.number1
		int number2 = memory.number2
		int result = 0;
		switch (memory.operation)
		{
		case '+':
			result = number1 + number2;
			break;
		case '-':
			result = number1 - number2;
			break;
		case '*':
			result = number1 * number2;
			break;
		case '/':
			result = number1 / number2;
			break;
		default:
			result = -1;
			break;
		}

		memory.result = result;
	}
};
```
입력받은 값들을 계산해주는 부분을 CPU라는 Component로 분류했다.

##### Monitor
```
class Monitor
{
public :
	void Print(Memory& memory)
	{
		cout << memory.number1 << memory.operation << memory.number2 << "=" << memory.result;

	}
};
```
계산한 결과를 출력해주는 부분을 Monitor라는 Component로 분류했다.

##### Computer
```
class Computer
{
private :
	Memory memory;
	Keyboard keyboard;
	CPU cpu;
	Monitor monitor;

public :
	void ComputeAndPrint()
	{
		keyboard.Input(memory);
		cpu.Compute(memory);
		monitor.Print(memory);
	}
};
```
분류한 Component를 한 객체에 담아서 사용하면 된다!

다음과 같이 분리한다면 서로 커플링되던 코드를 디커플링할 수 있다. 예제는 간단힌 코드를 적은 것이기 때문에 효율이 없어보일 수 있지만 코드가 길어졌다고 가정한다면 큰 효과를 가져올 것이다. 또 재활용해야하는 코드가 많아질수록 더 효과를 볼 것이다. 이 블로그에서는 이 정도 수준의 분리를 하고 구조화를 했지만 Component 인터페이스를 만들어서 더 구조화 할 수도 있고 이를 활용해 Factory를 구축해 가독성과 성능을 향상시킬 수 도 있다.


&nbsp;&nbsp;&nbsp;&nbsp;


# 주의사항
---
Component의 기능을 제대로 분류하지 않는다면 오히려 더 복잡해질 가능성이 있다. 또 컴포넌트 간 통신이 어렵기 때문에 어떤 문제를 해결하기 위해서는 더 많은 단계를 거쳐야 할 가능성이 있다. 즉, 오버엔지니어링의 우려가 있다.

&nbsp;&nbsp;&nbsp;&nbsp;


# 개체와 Component간의 통신 문제점
---
사실 모듈을 디커플링하게 되면 따라오는 것이 바로 모듈간의 통신이다. 이 문제점으로 인해 위에서 언급한 오버엔지니어링이 발생할 수도 있다. 개체나 각 Component들은 서로 필요한 정보를 어떻게 얻어오고 서로를 참조할 수 있을까?

### 객체의 Component 참조
##### 객체가 필요한 컴포넌트를 스스로 생성
예를 들면 생성자에서 바로 컴포넌트를 생성하는 식으로 사용할 수 있다. 이렇게 되면 나중에 실수할 확률이 적어지지만 자유도가 떨어져 조합하는 방식이 늘어날 때 마다 새로운 클래스를 만들어야 한다.

```
class Computer
{
public :
    Computer()
    {
        memory = new Memory();
        ...
    }
};
```

##### 외부에서 컴포넌트 전달
만약 위 예시에서 Computer의 종류가 늘어나고 다른 부품을 갈아끼는 식의 기능이 추가된다하면 외부에서 전달하는 방식으로 해결할 수 있다. 이는 더 유연해져서 다양하게 사용할 수 있지만 잘못 조합할 확률이 있다.
```
class Computer
{
public :
    Computer(SubMemory& inputMemory ...)    //Memory를 상속받아 SubMemory를 만들어 넣을 수 있다.
    {
        memory = inputMemory;
        ...
    }
};
```

### Component끼리의 참조
##### 객체의 상태를 변경
컴포넌트들을 포함하는 객체에 컴포넌트들이 공유해야할 데이터를 모두 담는 것이다. 이는 컴포넌트들이 모두 디커플링상태를 완벽히 유지할 수 있어서 좋치만 공유해야하는 데이터를 전부 객체에 담아야해서 너저분해질 수 있고 메모리를 낭비하게 될 수도 있다.
```
class Computer
{
private:
    int data1;
    int data2;
    ...

private:
    Keyboard keyboard;
    Monitor monitor;
    ...

public:
    void ComputeAndPrint()
    {
        ...
        keyboard.Input(data1, data2);
        ...
        monitor.Print(data1, data2);
        ...
    }
};
```

##### Component들이 서로 직접 참조
앞서 보인 예시가 Memory를 참조하는 방식이다. 장점은 간단하고 빠르게 작업할 수 있고 명확한 작업이 가능하지만 역시나 이렇게 되면 결합도가 높아지게 된다..


##### 메시징
컨테이너 객체에 메세징 시스템을 만든 뒤 컴포넌트들에게 뿌리는 방식이다. 예전에 유니티를 활용해 게임프레임워크를 만들 때 이 방식을 사용해봤다. 결론적으로는 완벽한 디커플링이 가능하고 사용법도 까다롭지 않아서 나쁘지는 않았지만 매 컨텐츠가 추가될 때 마다 메세지를 추가하고 파싱해야했었고 디버깅할 떄 살짝 불편하다는 단점이 있었다.


&nbsp;&nbsp;&nbsp;&nbsp;


# 실제 예시
---
### Unity
Unity는 Component Pattern을 잘 반영했다. GameObject라는 객체에 Component들을 붙여서 사용할 수 있도록 설계되어있다. 따라서 디커플링을 아주 잘 할 수 있고 GetComponent라는 메서드를 통해 직접 다른 Component를 참조해서 사용할 수 있다.

&nbsp;&nbsp;&nbsp;&nbsp;

### React.js
최근 웹개발에 흥미가 생겨서 React를 간단히 공부했었다. 구조가 Unity와 약간 유사하다는점이 흥미로웠다(나중에 Unity 개발자가 React를 익히는 법에 대해서도 포스팅할 기회가 생기게 된다면 포스팅하고 싶다). React에서도 React.Component를 상속받아 클래스를 만들고 DOM에 뿌려주는 방식이었다. 한가지 특이한 점은 데이터 관리를 prop과 state로 한다는 점이었는데 앞서 설명한 '객체의 상태를 변경'과 '메시징'을 혼합한 것 처럼 보였다. 

---
