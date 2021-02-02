---
title : "[DesignPattern]Observer Pattern"
layout: post
author: Kim heetae
tag : [DesignPattern]
---

# Observer Pattern
---
어떤 객체(Subject)의 상태 변화가 발생할 때 다른 특정한 객체들(Observer)이 영향을 받는 경우가 많을 것이다. 이 때 어떤 객체(Subject)를 주체라 하고 다른 특정한 객체들(Observer)을 관찰자 라고 칭하겠다. 주체의 상태 변화에 대하여 관찰자의 반응을 구현하고 싶을 때 서로의 코드가 관여하게 되어 커플링이 발생해 결합도가 올라가게 된다. 이를 방지하게 위해 주체의 상태의 변화를 관찰자에게 통보해주는 기법이 관찰자 패턴이다.



&nbsp;&nbsp;&nbsp;&nbsp;



# 구현
---
캐릭터가 점프 입력(Space Bar)를 받았을 경우에 튜토리얼매니저와 업적매니저가 각각 상태를 업데이트하는 예제를 만들어보겠다. 본 예제는 Unity로 작성했고 Unity 특성에 맞게 구성했다. 이 글을 참고하여 관찰자 패턴을 구현하는 경우 본인의 개발환경에 맞게 재구성하면 된다.

먼저 관찰자 패턴의 틀을 구성하자!

### IObservable.cs
관찰자가 구현할 interface이다. C#으로 구현했기 떄문에 Generic을 사용하여 전달할 파라미터에 entity와 event를 다음과 같이 구성했다.
```
public interface IObservable<ObjectType,EventType>
{
    void OnNotify(ObjectType entity, EventType notiEvent);
}
```

### ObserverMonoBehaviour.cs
Unity로 제작했기 떄문에 MonoBehaviour를 상속받아 구현했다. 꼭 클래스로 만들 필요는 없지만 필자는 예제의 이해를 돕기위해 다음과 같이 구현했다.
```
public class ObserverMonoBehaviour<ObjectType, EventType> : MonoBehaviour
{
    private List<IObservable<ObjectType, EventType>> observerList;

    public void AddObserver(IObservable<ObjectType, EventType> newObserver)
    {
        // Lazy initiallize
        if(observerList == null)
        {
            observerList = new List<IObservable<ObjectType, EventType>>();
        }

        if(observerList.Contains(newObserver))
        {
            return;
        }
        else
        {
            observerList.Add(newObserver);
        }
    }

    public void RemoveObserver(IObservable<ObjectType, EventType> removeObserver)
    {
        if (observerList.Contains(removeObserver))
        {
            return;
        }
        else
        {
            observerList.Add(removeObserver);
        }
    }

    protected void Notify(ObjectType entity, EventType notiEvent)
    {
        foreach(var observer in observerList)
        {
            observer.OnNotify(entity, notiEvent);
        }
    }
}
```
이제 주체가 될 Character를 만들고 입력을 받으면 Notify를 실행할 것이고 튜토리얼매니저와 업적매니저는 Notify를 받아 로그를 출력할 것이다.

### Character.cs
```
public class Character : ObserverMonoBehaviour<GameObject, string>
{
	// Use this for initialization
	void Start ()
    {
        AddObserver(TutorialManager.Instance);
        AddObserver(AchievementManager.Instance);
    }

	// Update is called once per frame
	void Update ()
    {
	    if(Input.GetKeyDown(KeyCode.Space))
        {
            Notify(this.gameObject, "Jump");
        }
	}
}
```

### TutorialManager.cs
```
public class TutorialManager : IObservable<GameObject, string>
{
    public static TutorialManager Instance = new TutorialManager();

    public void OnNotify(GameObject entity, string notiEvent)
    {
        Debug.Log(entity.name + notiEvent);
    }
}
```

### AchievementManager.cs
```
public class AchievementManager : IObservable<GameObject, string>
{
    public static AchievementManager Instance = new AchievementManager();

    public void OnNotify(GameObject entity, string notiEvent)
    {
        Debug.Log(entity.name + notiEvent);
    }
}
```
구현하는 방법은 자신의 기호에 따라 바꾸면 된다. Character는 Start()에서 각각의 관찰자(업적매니저와 튜토리얼매니저)들을 등록한다. 그리고 입력(Space Bar)을 받으면 관찰자들에게 entity와 event를 Notify해준다. Notify를 받은 관찰자들은 각각의 로직을 수행하면 된다!




&nbsp;&nbsp;&nbsp;&nbsp;




# 주의사항
---
관찰자 패턴은 좋아보이지만 주의해야할 항목들이 몇가지 있다. 이 항목들은 각 객체들을 삭제하는 경우에 발생한다.

### Dangling Poiter
구현하는 방법에 따라 다르겠지만 C++로 작성할 때 관찰자들을 포인터로 들고있는 경우 관찰자를 부주의하게 삭제하는 경우 Dangling Pointer가 발생할 수 있다.


### 관찰자의 대기
관찰자가 주체의 이벤트를 기다리고 있어야하는 경우가 있을 것이다.(사실 이런경우는 관찰자 패턴은 어울리지 않는다고 생각함) 이 때, 주체를 삭제한다면 관찰자는 주체의 메세지를 계속해서 기다리게 될 것이다. 이는 주체가 삭제될 때 삭제 이벤트를 전송하고 그에 맞는 로직을 구현해서 해결할 수 있다.


### 사라진 리스너 문제
관찰자를 제대로 삭제해주지 않는 경우이다. GC를 탑재하고 있는 언어에서 알림 시스템을 개발하는 경우 빈번히 발생하는 문제이다. 주체가 관찰자들을 등록해서 레퍼런스를 물고있는데 관찰자를 등록취소없이 오브젝트를 삭제한다면 실제 객체는 GC가 수거해갈 수 없다. 따라서 메모리 누수가 발생할 것이고 또 등록되어있는 만큼 cpu 낭비가 발생할 것이며 같은 관찰자를 등록할 때 마다 사용되지는 않으면서 중복으로 등록이 될 것이다.



&nbsp;&nbsp;&nbsp;&nbsp;



# 한계점과 해결법
---
예제 코드는 잘 구동된다. 하지만 몇가지 한계점을 갖고있다. 큰 문제점은 아니지만 상황에 따라 약간의 더 좋은 효과를 낼 수 있는 해결방법을 소개하겠다.

### Blocking Code
주체가 Notify를 할 때에는 자신에게 등록된 관찰자들의 OnNotify를 하나씩 호출하게 된다. 여기서 만약 한 관찰자가 Notify를 받았을 경우 상당한 양의 로직을 수행하는 경우 이 코드는 Block 될 것이다.
문제되지 않을 것 처럼 보이지만 코드들이 서로 디커플링되어있고 결합도가 낮아 신경쓰지 않아도 된다면 충분히 실수할 수 있는 부분이라고 본다.

#### 해결법
당연 Blocking Code를 해결하는 방법은 NonBlock Code로 변경하는 것이다. 그렇기 위해서는 Thread를 사용하거나 Event Queue에 담아서 사용하면 된다. 단 당연하지만 MultiThread와 Lock을 사용할 경우에는 교착상태에 빠지지 않도록 주의해야한다

&nbsp;&nbsp;&nbsp;&nbsp;

### 동적할당을 해주는 컬렉션 사용
GC가 탑재되어있는 언어에서 제공되는 컬렉션(리스트 혹은 해쉬 등)을 사용하면 Add, Remove 시 내부적으로 메모리 할당에 관여되도록 되어있다. Object Pool Post에서 해당하는 내용을 다루고 있다. 사실 큰 문제는 아니지만 이를 최소화한다면 더 나은 성능이 될 것이다.
예제는 C#으로 구현되었는데 주체에서는 관찰자를 System.Collection.Generic에서 제공되는 List<T>를 사용하고 있다. 많은 양의 동적할당을 하는 것은 아니지만 최적을 위해 개선해보겠다.

#### 해결법
C#에서 제공되는 컬렉션을 사용하지 않고 Linked List로 바꿔서 만들어보겠다.

#### Observable.cs를 interface에서 class로 변환
class로 변환하고 nextObserver를 레퍼런스로 들고있게 한다.
```
public abstract class Observable<ObjectType,EventType>
{
    internal Observable<ObjectType, EventType> nextObserver;

    public abstract void OnNotify(ObjectType entity, EventType notiEvent);
}
```

#### ObserverMonoBehaviour.cs에서는 List<T> 대신 LinkedList의 head로 변환
head만 들고있고 Add할 시 레퍼런스를 연결해준다. 속도를 위해 가장 앞에 연결해준다. 제거할때는 순회하여 제거한다.
```
public class ObserverMonoBehaviour<ObjectType, EventType> : MonoBehaviour
{
    private Observable<ObjectType, EventType> head;

    public void AddObserver(Observable<ObjectType, EventType> newObserver)
    {
        if (head == null)
        {
            head = newObserver;
        }
        else
        {
            newObserver.nextObserver = head;
            head = newObserver;
        }
    }

    public void RemoveObserver(Observable<ObjectType, EventType> removeObserver)
    {
        if(head == null || removeObserver == null)
        {
            return;
        }

        Observable<ObjectType, EventType> current = head;        

        if(current == removeObserver)
        {
            head = current.nextObserver;
        }

        while (current.nextObserver != null)
        {
            if(current.nextObserver == removeObserver)
            {
                LinkNextObserver(current);
                break;
            }

            current = current.nextObserver;
        }
    }

    private void LinkNextObserver(Observable<ObjectType, EventType> currentObserver)
    {
        Observable<ObjectType, EventType> nextObserver = null;
        nextObserver = currentObserver.nextObserver;

        Observable<ObjectType, EventType> nextnextObserver = null;
        if (nextObserver != null)
        {
            nextnextObserver = nextObserver.nextObserver;
        }

        currentObserver.nextObserver = nextnextObserver;
    }

    protected void Notify(ObjectType entity, EventType notiEvent)
    {
        Observable<ObjectType, EventType> current = head;
        while (current != null)
        {
            current.OnNotify(entity, notiEvent);
            current = current.nextObserver;
        }
    }
}
```

추가로 관찰자를 직접 들고있기보다 관찰자를 담는 Container를 만들고 그 Container를 LinkedList로 만든 후 그 Container를 객체 풀에 만들어 관리하면(ObjectPool Pattern 포스트 참고) 관찰자를 클래스로 변환하지 않아도 되고 LinkedList에 관련된 코드를 밖으로 빼낼 수 있다.




&nbsp;&nbsp;&nbsp;&nbsp;



# 더 나은 방법
---
최근 함수형 프로그래밍의 유행으로 관찰자 패턴은 함수의 레퍼런스나 클로저, 콜백 등으로 구현을 많이 하게 되었다. 다음 예제는 C#의 event와 delegate를 사용해서 관찰자 패턴을 구현한 것이다.
관찰자가 될 객체들은 interface를 상속할 필요가 없어지고 주체가 될 객체는 Delegate만 들고있으면 된다.

### AchievementManager.cs
```
public class AchievementManager
{
    public static AchievementManager Instance = new AchievementManager();

    public void OnNotify(GameObject entity, string notiEvent)
    {
        Debug.Log(entity.name + notiEvent);
    }
}
```

### TutorialManager.cs
```
public class TutorialManager
{
    public static TutorialManager Instance = new TutorialManager();

    public void OnNotify(GameObject entity, string notiEvent)
    {
        Debug.Log(entity.name + notiEvent);
    }
}
```

### Character.cs
```
public class Character : MonoBehaviour
{
    public delegate void SomeEvent(GameObject entity, string notiEvent);
    private event SomeEvent OnEvent;

    void Start()
    {
        OnEvent += TutorialManager.Instance.OnNotify;
        OnEvent += AchievementManager.Instance.OnNotify;
    }

	void Update ()
    {
	    if(Input.GetKeyDown(KeyCode.Space))
        {
            OnEvent(this.gameObject, "Jump");
        }
	}
}
```

위에서 한 노력들이 모두 헛수고가 된다.. ㅠㅠㅠ 관찰자 패턴을 익히기 위한 노력이라고 생각하자! 죄송해요

첫 예제와 지금 작성한 예제에는 공통점이 있다.

1. 관찰자는 상태변화의 알림을 받는다.
2. 알림을 받으면 다른 상태를 변화시킨다.

이 두 가지가 바로 관찰자 패턴의 핵심이다. 현재에 와서는 dataflow programming이나 funtional reative programming 등 새로운 프로그래밍 패러다임들이 등장하면서 일종의 관찰자 패턴을 쉽게 코딩에 스며들게 했다.




&nbsp;&nbsp;&nbsp;&nbsp;



# 개인적인 의견
---
이 포스팅에서는 관찰자 패턴을 소개하고 구현 방법을 설명했다. 결국 현재의 프로그래밍 언어들의 기능과 새로운 프로그래밍 패러다임들이 이러한 노력없이 쉽게 관찰자 패턴이 스며드는 코딩을 할 수 있도록 설계되긴 헀지만 관찰자 패턴을 이해있다면 나중에 더 좋은 설계에 사용할 수 있을 것이라 생각한다. 위에서 여러가지 용어를 덧붙여 설명했지만 중요한건 최종 결론에서 설명한 핵심 2가지를 지킨다면 어떻게 구현해도 상관없다고 생각한다.

실제로 1:1 매칭되는 관계에 관찰자만 변경되는 관찰자 패턴을 도입한 경우도 봤다. 그러한 경우에 굳이 관찰자들을 리스트로 들고 있을 필요가 없을 것이며 이벤트를 딱히 지정하지 않아도 구현이 가능할 것이다. 관찰자 패턴의 핵심 2가지를 잘 지키고 있기에 다른 개발 시간이나 개발 자원을 투자하지 않고도 충분히 잘 동작하는 코드였다. 다른 디자인 패턴들과 같이 구현이나 용어는 패턴의 핵심을 도우는 도구인 것 같다. 이번 포스팅도 같은 개념으로 접근했으면 좋겠다.

&nbsp;&nbsp;&nbsp;&nbsp;


##### [예제 저장소는 이 링크에서 확인!][link]

[link]: https://github.com/kimheetae90/ObserverPattern
