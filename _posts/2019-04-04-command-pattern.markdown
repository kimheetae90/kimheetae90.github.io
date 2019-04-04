---
title: "Command Pattern"
layout: post
date: 2019-03-11 14:08
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Design Pattern
- Command Pattern
category: blog
author: heetaekim
description: Command Pattern의 소개와 사용법 및 이점
---

# Command Pattern
---
Command Pattern에 대해 조사하고 Command Pattern의 강점과 단점에 대해 생각해 본 결과를 포스팅한다. Command pattern은 명령, 요청 혹은 실행되어야 할 기능 등을 캡슐화 하는 기법으로 여러가지 기능을 재사용 할 수 있게 해주고 요청을 대기시키거나 되돌릴 수 있는 연산을 지원하는 방법이다. 언어에 따라서 인터페이스, 콜백, 일급함수, 함수포인터, 클로저, 람다 등으로 구현이 가능하다.

&nbsp;&nbsp;&nbsp;&nbsp;

# 도입 전 코드
---
예를 들어 어떤 오브젝트를 생성하고 이동시키는 프로그램을 만드는 상황을 마주했다고 치자! 이 프로그램은 툴이 될 수도 있고 게임이 될 수도 있을 것이다. 이 때 키 입력을 받고 그에 상응하는 액션을 바로 함수로 호출한다면 아래와 같을 것이다. (이 포스팅의 예제는 Unity에서 작성)

### Character.cs
생성하고 이동할 오브젝트
```
public class Character : MonoBehaviour
{
    public void Move(Direction direction)
    {
        Vector3 moveVector;

        switch (direction)
        {
            case Direction.Back:
                moveVector = new Vector3(0, 0, -1);
                break;
            case Direction.Front:
                moveVector = new Vector3(0, 0, 1);
                break;
            case Direction.Left:
                moveVector = new Vector3(-1, 0, 0);
                break;
            case Direction.Right:
                moveVector = new Vector3(1, 0, 0);
                break;
            default:
                moveVector = new Vector3(0, 0, 0);
                break;
        }

        this.transform.Translate(moveVector);
    }
}
```

### BlockManager.cs
키 입력을 받으며 오브젝트를 관리하는 매니저
```
public class BlockManager : MonoBehaviour {

    public static BlockManager Instance;

    public Character SelectedCharacter { get; private set; }
    
    public void Awake()
    {
        Instance = this;
    }

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.J))
        {
            CreateCharacter();
        }
        else if (Input.GetKeyDown(KeyCode.K))
        {
            RemoveCharacter();
        }
        else if (Input.GetKeyDown(KeyCode.A))
        {
            MoveSelectCharacter(Direction.Left);
        }
        else if (Input.GetKeyDown(KeyCode.S))
        {
            MoveSelectCharacter(Direction.Back);
        }
        else if (Input.GetKeyDown(KeyCode.D))
        {
            MoveSelectCharacter(Direction.Right);
        }
        else if (Input.GetKeyDown(KeyCode.W))
        {
            MoveSelectCharacter(Direction.Front);
        }
    }

    public Character CreateCharacter()
    {
        GameObject prefab = Instantiate(Resources.Load("Character") as GameObject) as GameObject;
        Select(prefab.GetComponent<Character>());
        return SelectedCharacter;
    }

    public void Select(Character character)
    {
        SelectedCharacter = character;
    }

    public void RemoveCharacter()
    {
        SelectedCharacter.gameObject.SetActive(false);
    }

    public void MoveSelectCharacter(Direction direction)
    {
        SelectedCharacter.Move(direction);
    }
}
```
위 코드에서는 키입력을 받은 경우 Update에서 확인하고 매니저 자체에서 생성한 오브젝트를 이동하는 예제이다. 문제점이 보이는가?
&nbsp;
물론 위와 같은 코드도 충분히 기능한다. 하지만 프로젝트의 규모가 커지면서 개발 인원이 늘어나서 각자 다른 기능을 구현하게되거나, 실행한 기능,명령을 뒤로 되돌려야 하는 상황 또는 키 맵핑 기능 추가 등을 마주한 경우에는 어떻게 개발할 것인가!
&nbsp;
개발 인원이 많아지는 경우엔 한 스크립트를 공유하며 작업하기 때문에 Merge에 경우 문제가 될 것이다(물론 이를 해결할 수 있는 다른 방법은 많다). 하지만 Undo기능이나 키 맵핑 기능은 이 구조에서는 쉽게 해결할 수 없다.


&nbsp;&nbsp;&nbsp;&nbsp;
# 패턴 도입
---
이 때 도입할 수 있는 방법이 Command Pattern이다. 위키백과에서 Command Pattern은 명령(command), 수신자(receiver), 발동자(invoker), 클라이언트(client) 라는 용어로 구분할 수 있다고 설명하고 있다. 하지만 나는 Command를 캡슐화 한다는 규칙만 적용한다면 이 네 개를 모두 구현할 필요가 없이 더 자유롭게 사용할 수 있다고 생각한다.

먼저 Command의 형태를 보면 다음과 같다. 
### ICommand.cs
```
public interface ICommand
{
    void Execute();
    void Undo();
    void Dispose();
    void Redo();
}
```
Execute는 해당 Command를 수행하는 함수이다. Undo와 Redo는 기능에 따라서 넣으면 되는 함수이고 Dispose같은 경우는 Command가 소멸될 때, 게임에서 Scene이나 State가 변경되어 해당 Command가 사용되지 않을 때 마무리 작업할 때 사용하라고 작성해두었다(일종의 소멸자). 그리고 BlockManager가 수행하고 있는 기능들을 Command로 뽑아서 작성해보자!

### CreateCommand.cs
오브젝트를 생성하는 명령
```
public class CreateCommand : ICommand {

    GameObject prefab = null;
    Character instance = null;

    public CreateCommand(string path)
    {
        prefab = Resources.Load(path) as GameObject;
    }

    public void Dispose()
    {
        prefab = null;
    }

    public void Execute()
    {
        GameObject go = GameObject.Instantiate(Resources.Load("Character") as GameObject) as GameObject;
        instance = go.GetComponent<Character>();
        BlockManager.Instance.Select(instance);
    }

    public void Undo()
    {
        instance.gameObject.SetActive(false);
    }    

    public void Redo()
    {
        instance.gameObject.SetActive(true);
        BlockManager.Instance.Select(instance);
    }
}
```


### MoveCommand.cs
오브젝트를 움직이는 명령
```
public class MoveCommand : ICommand {

    private Direction direction;
    private Character character;

    public MoveCommand(Direction direction, Character character)
    {
        this.direction = direction;
        this.character = character;
    }

    public void Dispose()
    {
        character = null;
    }

    public void Execute()
    {
        character.Move(direction);
    }

    public void Undo()
    {
        int opposition = ((int)direction + 2) % (int)Direction.Max;
        character.Move((Direction)opposition);
    }
    
    public void Redo()
    {
        character.Move(direction);
    }
}
```

### RemoveCommand.cs
오브젝트를 삭제하는 명령
```
public class RemoveCommand : ICommand
{
    private Character character = null;

    public RemoveCommand(Character character)
    {
        this.character = character;
    }

    public void Dispose()
    {
        character = null;
    }

    public void Execute()
    {
        character.gameObject.SetActive(false);
    }

    public void Undo()
    {
        character.gameObject.SetActive(true);
    }

    public void Redo()
    {
        character.gameObject.SetActive(false);
    }
}
```

BlockManager에는 관리자 자체의 기능만 남기고 나머지 기능은 Command Pattern으로 빼냈다. 따라서 Command Pattern의 기본 요소에 해당되는 Invoker인 CommandManager를 만들자!

### CommandManager.cs
```
public class CommandManager : MonoBehaviour
{
    private Stack<ICommand> commandStack;
    private Stack<ICommand> undoStack;

    private ICommand reservedCommand;

    void Start()
    {
        commandStack = new Stack<ICommand>();
        undoStack = new Stack<ICommand>();
    }

    ICommand CreateCreateCommand(string path)
    {
        ICommand newCommand = new CreateCommand(path);
        ExecuteCommand(newCommand);
        return newCommand;
    }

    ICommand CreateRemoveCommand()
    {
        ICommand newCommand = new RemoveCommand(BlockManager.Instance.SelectedCharacter);
        ExecuteCommand(newCommand);
        return newCommand;
    }

    ICommand CreateMoveCommand(Direction direction)
    {
        ICommand newCommand = new MoveCommand(direction, BlockManager.Instance.SelectedCharacter);
        ExecuteCommand(newCommand);
        return newCommand;
    }

    void ExecuteCommand(ICommand command)
    {
        commandStack.Push(command);
        reservedCommand = command;
        ClearUnDoStack();
    }

    void UnDo()
    {
        if (commandStack.Count <= 0)
            return;

        ICommand undoCommand = commandStack.Pop();
        undoCommand.Undo();
        undoStack.Push(undoCommand);
    }

    void ReDo()
    {
        if (undoStack.Count <= 0)
            return;
        
        ICommand redoCommand = undoStack.Pop();
        redoCommand.Redo();
        commandStack.Push(redoCommand);
    }
    
    void ClearUnDoStack()
    {
        if(undoStack.Count > 0)
        {
            undoStack.Clear();
        }
    }

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.J)) CreateCreateCommand("Character");
        else if (Input.GetKeyDown(KeyCode.K)) CreateRemoveCommand();
        else if (Input.GetKeyDown(KeyCode.A)) CreateMoveCommand(Direction.Left);
        else if (Input.GetKeyDown(KeyCode.S)) CreateMoveCommand(Direction.Back);
        else if (Input.GetKeyDown(KeyCode.D)) CreateMoveCommand(Direction.Right);
        else if (Input.GetKeyDown(KeyCode.W)) CreateMoveCommand(Direction.Front);
        else if (Input.GetKeyDown(KeyCode.N)) UnDo();
        else if (Input.GetKeyDown(KeyCode.M)) ReDo();

        if (reservedCommand != null)
        {
            reservedCommand.Execute();
            reservedCommand = null;
        }

    }
}
```
다음과 같이 작성한다면 Input을 할 때 마다 그 것에 맞는 Command를 생성할 것이고 그 Command를 Execute 할 것이다. 또 Command를 발동한 History를 Stack으로 남긴다면 Undo기능을 만들수도 있다. 위 코드는 Redo코드까지 작성했다. 
&nbsp;
나는 위와 같은 구조로 작성했지만 더 좋은 방법으로 Invoker를 작성할 수도 있다. 예를 들면 Input과 실행될 Command의 종류를 Dictionary로 관리하여 간단한 키 맵핑 기능을 만든다던가, 이미 사용되었던 Command를 Caching해서 성능을 향상시키는 등의 코드를 작성할 수 있다. 이 포스트에서는 다루지 않겠다.


&nbsp;&nbsp;&nbsp;&nbsp;
# 생각되는 문제점
---
Command Pattern은 명령을 추상화하여 각 기능끼리 디커플링하고 관리자와 떨어뜨려 제작하는 방법이기에 결국엔 Event와 유사하게 Invoke되는 방식이다. 
* 이 때 주의할 점은 Invoke된 Command(A)에서 또 다른 Command(B)를 Invoke하고 다시 기존의 Command(A)를 Invoke하는 경우가 생길 수 도 있다. 다음과 같은 경우엔 무한루프로 이어지게 된다.
*또, Command 클래스에서 기능 수행에 필요한 객체의 레퍼런스를 Caching해서 사용하는데 이 때 실제 객체가 사라지고 나서 기능이 수행된다면 NullReference가 발생할 것이다. 이 상황에 대한 예외처리도 대비해야 할 것이다.
*그리고 GC를 탑재하고 있는 언어의 경우에서는 사용이 완료된 Command를 제대로 정리하지 않는다면 Command 클래스에서 사용할 때 Caching한 Reference에 대해서는 GC가 수거해가지 않을 것이기 때문에 메모리누수가 발생할 수 있다.

&nbsp;
따라서 약한참조를 사용하는 등의 대비를 잘 해야할 것이라 생각된다.

&nbsp;&nbsp;&nbsp;&nbsp;
# 결론
---
물론 처음의 코드가 직관적이고 코드가 짧고 가독성이 좋고 간결할 수 있다. 하지만 Command Pattern을 사용한다면 작업자들끼리 분할하여 일을 할 수 있고 Undo기능, 나아가 Redo기능까지 손쉽게 개발할 수 있다. 특히 Tool을 제작할 때 강력하게 작용할 것이다.