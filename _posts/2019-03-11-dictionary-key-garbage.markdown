---
title: "Dictionary Key Garbage"
layout: post
date: 2019-03-11 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- dictionary
- key
- garbage
- enum
category: blog
author: heetaekim
description: dictionary를 사용할 때 key가 enum이나 class로 되어있는 경우 가비지가 발생하는데 이를 방지해보자!
---

# [C#] Dictionary의 Key를 Enum, Class타입으로 사용할 때 GC호출 방지하기!
---
---

### Dictionary의 Key 비교
---
Dictionary는 Key와 Value를 갖는 컬렉션이다. Dictionary<Key,Value>의 형태로 사용한다. 이 때 Key에 대한 일치 여부를 판별할 때에는 IEqualityComparer<T>라는 제네릭 인터페이스를 사용할 수 있다. 이 인터페이스를 구현하지 않으면 EqualityComparer<T>.Default라는 EqualityComparer<T> 클래스의 기본 인스턴스를 사용한다. EqualityComparer<T>.Default에서는 IEquatable<T>.Equals를 사용해서 비교를 한다. 이 때 Object.Equals와 Object.GetHashCode를 사용한다.

cf) MS Doc에서는 IEqualityComparer<T>를 구현하기보다 EqualityComparer<T> 클래스를 상속해서 재정의하는 방법으로 사용하는 것을 권장하고 있다.
cf) IEqualityComparer<T>는 equality comparisons만 제공! sorting and ordering은 IComparer<T>를 구현해야 함!

### 문제점
---
Key를 비교할 때에는 Equals(Object)를 사용하게 되는데 int 같은 경우는 System.Int32 구조체에 IEquatable<Int32>가 구현되어있어 Key를 비교할 때 가비지가 발생하지 않는다. 하지만 Enum타입이나 사용자가 만든 struct같은 경우는 IEquatable<T>를 구현하지 않아서 IEquatable<T>.Equals를 사용할 때 Objects.Equals(Object) 가 사용되게 되는데 이 때 boxing이 일어난다!

### 가비지 발생 확인
---
Dictionary의 Key를 int와 Enum으로 구분하여 자주 사용되는 메서드를 호출해보고 가비지 발생량을 Unity에서 작성하고 Unity Profiler를 통해 체크해봤다.

##### 1. Init
 
``` 
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

enum TestEnum
{
    A,
}

public class DictionaryEnumTest : MonoBehaviour
{
    Dictionary<TestEnum, string> testEnumDic;
    Dictionary<int, string> testIntDic;
    
    void Start()
    {
        InitEnumDic();
        InitIntDic();
    }

    void InitEnumDic()
    {
        testEnumDic = new Dictionary<TestEnum, string>();
    }

    void InitIntDic()
    {
        testIntDic = new Dictionary<int, string>();
    }
}
``` 

![Dictionary 인스턴스 생성 시 가비지 량](/asset/image/post/2019-03-12-Dictionary-Key-Garbage/2019-03-12-init-garbage.jpg)

Enum 타입을 Key로 한 Dictionary의 가비지량이 0.2KB 정도 더 많다

##### 2. Add
 
``` 
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

enum TestEnum
{
    A,
}

public class DictionaryEnumTest : MonoBehaviour
{
    Dictionary<TestEnum, string> testEnumDic;
    Dictionary<int, string> testIntDic;
    
    void Start()
    {
        InitEnumDic();
        InitIntDic();
        AddEnumDic();
        AddIntDic();
    }

    void InitEnumDic()
    {
        testEnumDic = new Dictionary<TestEnum, string>();
    }
    
    void InitIntDic()
    {
        testIntDic = new Dictionary<int, string>();
    }
    
    void AddEnumDic()
    {
        testEnumDic.Add(TestEnum.A, "A");
    }

    void AddIntDic()
    {
        testIntDic.Add(1, "A");
    }
}
``` 

![Add시 가비지 량](/asset/image/post/2019-03-12-Dictionary-Key-Garbage/2019-03-12-add-garbage.jpg)
int dictionary의 경우는 가비지가 발생하지 않았지만 enum dictionary는 20B의 가비지가 발생했다.

##### 3. TryGetValue
 
``` 
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

enum TestEnum
{
    A,
}

public class DictionaryEnumTest : MonoBehaviour
{
    Dictionary<TestEnum, string> testEnumDic;
    Dictionary<int, string> testIntDic;

    void Start()
    {
        InitEnumDic();
        InitIntDic();
        AddEnumDic();
        AddIntDic();
        GetValueEnumDic();
        GetValueIntDic();
    }

    void InitEnumDic()
    {
        testEnumDic = new Dictionary<TestEnum, string>();
    }
    
    void InitIntDic()
    {
        testIntDic = new Dictionary<int, string>();
    }

    void AddEnumDic()
    {
        testEnumDic.Add(TestEnum.A, "A");
    }

    void AddIntDic()
    {
        testIntDic.Add(1, "A");
    }

    void GetValueEnumDic()
    {
        string result;
        testEnumDic.TryGetValue(TestEnum.A, out result);
    }

    void GetValueIntDic()
    {
        string result;
        testIntDic.TryGetValue(1, out result);
    }
}
``` 

![Dictionary 인스턴스 생성 시 가비지 량](/asset/image/post/2019-03-12-Dictionary-Key-Garbage/2019-03-12-trygetvalue-garbage.jpg)
int dictionary의 경우는 가비지가 발생하지 않았지만 enum dictionary는 60B의 가비지가 발생했다.

TryGetValue는 가장 빈번할 것으로 예상되는 메서드이다. 실제로 다음과 같은 경우는 거의 없겠지만 이 메서드를 10만번 돌려봤다.
![TryGetValue 시 가비지 량](/asset/image/post/2019-03-12-Dictionary-Key-Garbage/2019-03-12-trygetvalue10-garbage.jpg)

약 5.7MB의 가비지가 발생했다.

### 해결 방안!
---
위의 설명처럼 Key에 대한 일치 여부를 판별할 때에는 IEqualityComparer<T>라는 제네릭 인터페이스를 사용할 수 있다. 그렇게 되면 EqualityComparer<T>.Default를 사용하지 않고 IEqualityComparer<T>를 구현한 메서드를 사용할 것이다. IEqualityComparer<T>를 구현하여 비교할 때 사용될 클래스를 작성하자!

``` 
public struct MyEnumCOmparer : IEqualityComparer<MyEnum> 
{
    public bool Equals(MyEnum x, MyEnum y) 
    {
        return x == y; 
    }
    
    public int GetHashCode(MyEnum obj)
    {
        return (int)obj; 
    }
}
``` 

이 구조체를 dictionarty의 생성자에 넣어주면 위에서 의도한 방향으로 진행된다.

``` 
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public enum TestEnum
{
    A,
}

public struct TestEnumComparer : IEqualityComparer<TestEnum>
{
    public bool Equals(TestEnum x, TestEnum y)
    {
        return x == y;
    }

    public int GetHashCode(TestEnum obj)
    {
        return (int)obj;
    }
}

public class DictionaryEnumTest : MonoBehaviour
{
    Dictionary<TestEnum, string> testEnumDic;
    void Start()
    {
        InitEnumDic();
        AddEnumDic();
        GetValueEnumDic();
    }

    void InitEnumDic()
    {
        testEnumDic = new Dictionary<TestEnum, string>(new TestEnumComparer());
    }

    void AddEnumDic()
    {
        testEnumDic.Add(TestEnum.A, "A");
    }

    void GetValueEnumDic()
    {
        string result;
        for(int i=0;i<100000;i++)
            testEnumDic.TryGetValue(TestEnum.A, out result);
    }    
}
``` 

![수정 결과](/asset/image/post/2019-03-12-Dictionary-Key-Garbage/2019-03-12-result-garbage.jpg)

TryGetValue를 10만번 한 코드에서 가비지가 발생하지 않았다!

이미 작업된 프로젝트에서 가비지를 제거하기 위해 enum이나 struct가 key로 된 dictionary를 모두 int로 바꾸는 일은 불가능에 가깝다.. 위와 같은 방법을 사용하면 비교적 적은 작업량으로 가비지 생성을 방지할 수 있다!


