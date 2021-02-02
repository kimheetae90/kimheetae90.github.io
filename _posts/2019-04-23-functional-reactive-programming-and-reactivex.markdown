---
title : "[Programming]Functional Reactive Programming과 ReactiveX"
layout: post
author: Kim heetae
tag : [Programming]
---

# Functional Reactive Programming을 접하다!
---
게임 개발만 하다가 웹에 관심을 돌리던 중 RxJS라는 것을 알게되었다. 최근 웹 개발 언어로 주로 사용되고 있는 JavaScript에서 사용되는 프레임워크들이 이 라이브러리를 많이 도입했다는 이야기를 듣고 공부하고 사용할 필요성을 느끼게 되어 공부하게 되었다.

RxJS는 JavaScript에서 사용하는 라이브러리로 FRP의 개념을 사용해 개발할 수 있도록 해주는 모듈이다.
사실 Rx는 3년전 UniRx를 통해 접해본 적이 있었다. 하지만 FRP에 대한 자세한 지식도 없고 Unite Seoul에서 발표된 PPT만 보고 사용해봤었기 때문에 엉망으로 사용했고 그 효율도 체감하지 못했다.

FRP에 대해 알아보기 위해 웹서핑을 많이 하던 도중 FRP를 이해하기 위해서는 Functional Programming과 Reactive Programming을 알아야하는 것을 알게 되었고 이 두 개념에 대해 조사해보았다.


&nbsp;&nbsp;&nbsp;&nbsp;

### Functional Programming
---
Functional Programming과 Reactive Programming은 새로운 프로그래밍 패러다임이다. 먼저 Functional Programming부터 알아보도록 하자!

Functional Programming은 기존에 개발하던 방식인 Imperative Programming와의 차이를 이해해야한다. Imperative Programming은 기존에 내가 게임을 개발할 때 사용하던 언어인 C#을 포함해 C++, Java 등이 OOP 패러다임을 사용하기위해 디자인된 언어의 개발방식이다. 같은 방식으로는 절차지향 프로그래밍이 있고 C언어가 이를 위해 디자인되었다.

Imperative Programming은 목표를 이루기 위해 상태와 그 상태를 변경시키는 로직에 중점을 두는 프로그래밍 방식이다. 즉, 알고리즘에 집중하게 된다.(알고리즘 프로그래밍이라고도 부른다고 한다.) 따라서 클래스나 구조체 등 인스턴스를 루프, 조건문을 사용해 상태를 제어해 알고리즘을 명시한 따라서 상태변경이나 실행 순서가 중요하다.
Functional Programming은 알고리즘보다 원하는 정보와 필요한 정보를 위해 함수로 문제를 구성하는 프로그래밍 방식이다. 따라서 일급 개체와 데이터컬렉션인 함수를 재귀를 비롯한 함수호출을 통 제어해 상태 변경은 있으면 안되고 실행 순서는 중요도가 낮은 편이다.

Functional Programming은 세가지 특징을 갖고 있다.

##### 1. 순수 함수
순수 함수는 side effect가 없는 함수로 thread-safe해서 병렬적 계산이 가능한 함수이다.

```
// 순수 함수의 예
int Sum(int x, int y)
{
  return x+y;
}
int a = 1, b = 2, c = 3;
var final = Sum(a,b) * Sum(a,c);


// 순수 함수가 아닌 경우
int Sum(int value, int addNumber)
{
  return value += addNumber; //값을 바꾸기 때문에 SideEffect 발생
}
int a = 1, b = 2, c = 3;
var final = Sum(a,b) * Sum(a,c);
```

##### 2. 익명 함수
함수는 ReturnValue Name(Parameter)의 형태로 정의되어서 이름이 있어야하지만 람다식을 통해 이름을 없앨 수 있다
```
Func<int,int> Square = (x)=> x * x;
Console.WriteLine(Square(5)); // 25
```

##### 3. 고계 함수
함수를 다루는 함수로써 함수 자체도 값이 되는 함수이다. 함수를 파라미터로 전달할 수도 있고 반환값으로 함수를 갖을 수도 있다.
```
List<int> list = new List()
list.Add(4);
list.Add(1);
list.Add(3);

list.Sort((x,y) => { return x.CompareTo(y); });
```


&nbsp;&nbsp;&nbsp;&nbsp;

### Reactive Programming
---
Reactive Programming은 비동기적 데이터흐름을 처리하는 프로그래밍 기법이다. 사용자 입장에서 본다면 실시간으로 바로 반응하는 프로그램이다. 익숙하지 않은가? 관찰자 패턴(Observer Pattern)이다. Data Binding을 통해 어떠한 Event를 비동기적으로 바로 처리하는 기법이 바로 Reactive Programming이다.

Reactive Programming에는 Data Stream이라는 개념이 존재한다. 이는 말그대로 데이터가 흐르는 관을 의미하고 발생하는 Event들을 시간 순으로 나열한 것이다. 이 Data Stream을 듣고있는 것을 Subscribing(구독)이라고 하고 Data Stream은 값, 에러 신호완료를 Emit(발생)시킬 수 있다.

다음 예시는 C#에서 구현되는 Observer Pattern의 예시이다. 더 자세한 내용은 관찰자 패턴 포스팅을 참조할 것!

##### 주체
```
public class Subject : IObservable<Event>
{
    private List<IObserver<Event>> observers;

    public Subject()
    {
        observers = new List<IObserver<Event>>();
    }

    public IDisposable Subscribe(IObserver<Event> observer)
    {
        if (!observers.Contains(observer))
            observers.Add(observer);
        return new Unsubscriber(observers, observer);
    }

    public class Unsubscriber : IDisposable
    {
        private List<IObserver<Event>> _observers;
        private IObserver<Event> _observer;

        public Unsubscriber(List<IObserver<Event>> observers, IObserver<Event> observer)
        {
            this._observers = observers;
            this._observer = observer;
        }

        public void Dispose()
        {
            if (!(_observer == null)) _observers.Remove(_observer);
        }
    }

    public void Send(Event value)
    {
        foreach(var observer in observers)
        {
            if (value == null)
                observer.OnError(new EventError());
            else
                observer.OnNext(value);
        }
    }

    public void Dispose()
    {
        foreach (var observer in observers.ToArray())
            if (observers.Contains(observer))
                observer.OnCompleted();

        observers.Clear();
    }
}

public class EventError : Exception
{
    internal EventError() { }
}
```

##### 관찰자
```
public class Observer : IObserver<Event>
{
    private IDisposable unsubscriber;
    private int id;

    public Observer(int id)
    {
        this.id = id;
    }

    public void OnCompleted()
    {
        Console.WriteLine("구독 끝!");
    }

    public void OnError(Exception error)
    {
        Console.WriteLine("에러");
    }

    public void OnNext(Event value)
    {
        Console.WriteLine(id + " get value " + value.arg);
    }

    public void Subscribe(IObservable<Event> provider)
    {
        unsubscriber = provider.Subscribe(this);
    }

    public void Unsubscribe()
    {
        unsubscriber.Dispose();
    }
}
```

##### Event
```
public class Event
{
    public string arg;
    public Event(string arg)
    {
        this.arg = arg;
    }
}
```

```
Subject subject = new Subject();

Observer observer1 = new Observer(1);
Observer observer2 = new Observer(2);

observer1.Subscribe(subject);
observer2.Subscribe(subject);

subject.Send(new Event("New Event!"));
subject.Dispose();
```



&nbsp;&nbsp;&nbsp;&nbsp;

### FRP
---
FRP(Functional Reactive Programming)은 이 두가지 개념을 섞은 것이다. Reactive Programming을 Functional Programming의 관점에서 구현하는 프로그래밍 기법이다. 즉 비동기적인 데이터 흐름을 처리할 때 함수적으로 처리하는 것이다.


&nbsp;&nbsp;&nbsp;&nbsp;

# ReactiveX
---
ReactiveX, 줄여 Rx는 Reactive Extensions의 약자로 여태까지 설명한 FRP의 개념을 사용할 수 있도록 구현해놓은 API의 집합체이다. ReactiveX 공식 페이지에서 FRP와 다르다고 설명하고 있다. FRP는 연속적인 데이터 변화의 대해 작동하는 반면, Rx는 불연속적으로 발생하는 데이터를 처리할 때 작동한다고 한다. Rx에서는 Data Stream을 Observable로 표현하고 있는데 더 자세한 내용은 차차 RxJS를 공부하며 포스팅하도록 하겠다.

Rx를 제대로 공부하지 않고 사용해봤기 때문에(UniRx) 제대로는 아직 사용해보진 않았지만 조사를 통해 발견한 ReactiveX의 장점은 코드의 간결함, 손쉬운 비동기처리, 콜백 지옥 탈출 등이 있었다. 단점으로는 역시 디버깅이었다.. 하지만 차차 이를 개선할 많은 방법들이 나올 것으로 보인다.

Rx를 사용하기 위해서는 Observable에 대해 이해해야하고 프로그램을 설계하고 구상하는 방법을 바꿔야한다고 한다. 또 기존에 Imperative하게 코드를 짜던 습관을 버리고 코딩 방식을 고쳐야한다고 한다. 그 동안 쌓여온 사고 방식을 한 순간에 고치는 것은 어렵겠지만 앞으로 RxJS를 공부하며 방식을 고치고 FRP에 더 익숙해져야 할 것이다. 더 나아가 RxJS 외의 RX들을 더 사용해보면 좋을 것 같다.
