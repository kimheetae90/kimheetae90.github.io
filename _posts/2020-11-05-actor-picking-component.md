---
title : "[UE4]Actor를 Picking하는 Component제작하기"
layout: post
author: Kim heetae
---

마우스 Input으로 특정 Actor를 Picking하는 Component를 제작해보겠다. 공부할 겸 모작을 하던 도중 Picking기능이 요구되서 R&D를 한 김에 포스팅을 하게 되었다. 목표는 Picking하는 Component를 만든 후 PlayerController에 생성하고 나서 컴포넌트 외부에서 원하는 로직을 붙일 수 있게 만드는 것이다.(이 포스팅은 Picking에 대한 로직을 다루지 않음.)


## 테스트 환경 세팅

Component를 제작하기 언리얼엔진에서 Picking을 하기 위해 프로젝트세팅에서 입력과 콜리전, PlayerController 세팅, 그리고 액터들을 레벨에 배치하는 등 세팅을 해줘야한다. 프로젝트를 하나 생성한 후 아래 단계에서 하나씩 알아보자. 이 포스팅에서는 UE4 4.25.4를 사용했다.

#### 1. 프로젝트 세팅

Input을 받기 위해서는 프로젝트 세팅에서 입력탭에서 액션을 매핑해줘야한다. 이 포스팅에서는 마우스 왼쪽 Press로 Actor를 집고 Release하면 놓아지는 구현을 할 것이다. 따라서 마우스 왼쪽버튼에 Picking이라는 액션을 생성하겠다.

!['Input Setting'](/assets/resource/2020-11-05-actor-picking-component/input.png)

다음은 Picking할 때 Actor를 식별하기위한 세팅인데 사실 이것은 구현하기 나름이다. 원한다면 Trace Channel말고 직접 LineTrace에 걸린 Actor의 프로퍼티를 검사해서 찾아낼 수도 있을 것이지만 이 포스팅에서는 Trace Channel을 사용했다. Pickable이라는 Channel을 생성한다. Pick을 원하는 Actor만 설정해줄 것이기 때문에 기본을 Ignore로 설정한다.

!['Trace Channel Setting'](/assets/resource/2020-11-05-actor-picking-component/tracechannel.png)

#### 2. PlayerController

다음은 PlayerController인데 유저가 직접 Picking을 하는 것이기 때문에 PlayerController에서 Input을 받아야한다. 그렇기 때문에 PlayerContoller를 상속받은 C++ 클래스를 하나 생성한 후에 SetupInputComponent에 바인딩을 해준다. 위에서 언급한 것 처럼 이 포스팅에서는 마우스 왼쪽 Press로 Actor를 집고 Release하면 놓아지는 구현을 할 것이기 때문에 그에 맞는 함수들도 만들어준다. 그리고 마우스포인터가 보여야하기 때문에 bShowMouseCursor를 true로 바꿔준다.

```c++
//PickingPlayerController.h 에 각각의 선언을 추가해주고 아래와 같이 정의한다.
void APickingPlayerController::SetupInputComponent()
{
	Super::SetupInputComponent();
	InputComponent->BindAction("Pick", IE_Pressed, this, &APickingPlayerController::OnMouseDown);
	InputComponent->BindAction("Pick", IE_Released, this, &APickingPlayerController::OnMouseUp);
}

void APickingPlayerController::BeginPlay()
{
	Super::BeginPlay();
	bShowMouseCursor = true;
}

void APickingPlayerController::OnMouseDown()
{
}

void APickingPlayerController::OnMouseUp()
{
}
```

안에 코드의 로직은 뒤에서 덧붙이도록 하겠다.

#### 3. 테스트할 액터 배치 및 카메라 세팅

처음 시작하면 기본 레벨이 띄워져있다. 여기에 자신이 원하는 액터를 씬에 한개 배치한다. 큐브를 배치해보겠다. 이 큐브는 Pick이 될 Actor이기 때문에 콜리전세팅에서 Pickable을 block으로 바꿔줘야한다.

!['Collision 세팅'](/assets/resource/2020-11-05-actor-picking-component/collisionsetting.png)

그리고 Picking을 할 때 기준이 될 카메라를 배치하겠다. 대충 TopView처럼 보이기위해 -75도정도 회전시키고 상단에 배치하겠다. 그리고 디테일창에서 Auto Player Activation에 Player 0번을 세팅해준다.

!['카메라 세팅'](/assets/resource/2020-11-05-actor-picking-component/cameradetail.png)

!['카메라 세팅 화면'](/assets/resource/2020-11-05-actor-picking-component/cameraplay.png)


## Component 기능 구현

이제 Component를 만들겠다. ActorComponent를 상속받은 Component를 하나 만든다. 먼저 Pick의 시작과 끝을 실행하는 함수를 만들어주겠다. Pick을 하기위해서는 위에서 정의한 TraceChannel을 사용해야하기 때문에 파라미터는 ECollisionChannel를 갖게 될 것이다. 또 시작을 했을 때 Picking을 실패했을 수 있기 때문에 반환을 bool형으로 지정해준다. 그리고 Pick한 Actor를 캐싱하고 있어야 할 것이다.

```c++
//PickActorComponent.h 에 선언

bool StartPick(ECollisionChannel inChannel = ECC_Visibility);
void EndPick(ECollisionChannel inChannel = ECC_Visibility);
	
AActor* PickedActor;
```

```c++
//PickActorComponent.cpp 에 정의

bool UPickActorComponent::StartPick(ECollisionChannel inChannel)
{
}

void UPickActorComponent::EndPick()
{
}
```

이 포스팅에서의 목표는 **Ray를 사용해서 Actor를 감지(목표1)**하고 **이 Actor의 정보를 유지해서 특정 액션을 취하는 것(목표2)**이다. **목표1**을 해결하기위해서는 카메라로부터 Viewport에서 Mouse의 위치에 해당하는 지점에 Ray를 쏘는 로직이 필요하다. 다행히도 언리얼엔진에서는 이 함수를 제공하고있다. PlayerContoller에 [GetHitResultUnderCursor()][GetHitResultUnderCursor]라는 함수이다. 예외처리를 포함한 로직을 StartPick에 구현해준다. 그리고 Pick이 끝날 때의 처리를 EndPick에 구현해준다.

```c++
bool UPickActorComponent::StartPick(ECollisionChannel inChannel)
{
	UWorld* world = GetWorld();
	if (!IsValid(world))
	{
		UE_LOG(LogTemp, Warning, TEXT("[PickActorComponent]StartPick Fail! World is Null."));
		return false;
	}

	APlayerController* playerController = world->GetFirstPlayerController();
	if (!IsValid(playerController))
	{
		UE_LOG(LogTemp, Warning, TEXT("[PickActorComponent]StartPick Fail! CachePlayerController is Null."));
		return false;
	}

	FHitResult outHit;
	if (playerController->GetHitResultUnderCursor(inChannel, false, outHit))
	{
		if (outHit.bBlockingHit)
		{
			PickedActor = outHit.GetActor();
			if (IsValid(PickedActor))
			{
				//Pick 할 시 실행할 로직을 만드세요

				UE_LOG(LogTemp, Warning, TEXT("[PickActorComponent]PickStart! Picked Object Name : %s."), *PickedActor->GetName());
				return true;
			}
		}
	}

	return false;
}

void UPickActorComponent::EndPick()
{
	if (IsValid(PickedActor))
	{
		UE_LOG(LogTemp, Warning, TEXT("[PickActorComponent]PickEnd! Picked Object Name : %s."), *PickedActor->GetName());
		// PickedActor를 null처리 하기 전 Pick을 끝낼 때 수행할 로직을 만드세요

		PickedActor = nullptr;
	}
}
```

이제 만들어준 Component를 PlayerController에서 생성해주고 기능을 연결해주자.

```c++
APickingPlayerController::APickingPlayerController()
{
	PickActorComponent = CreateDefaultSubobject<UPickActorComponent>(TEXT("PickActorComponent"));	
}

void APickingPlayerController::OnMouseDown()
{
	PickActorComponent->StartPick(ECC_GameTraceChannel1);
}

void APickingPlayerController::OnMouseUp()
{
	PickActorComponent->EndPick();
}
```

이제 컴파일을 한 후 큐브를 선택하면 출력창에 로그가 뜰 것이다.

!['Picking 확인'](/assets/resource/2020-11-05-actor-picking-component/picklog.png)


## Picking 중 Actor 이동시키기

사실 여기까지는 여태까지 작성한 것 처럼 복잡하기 작업하지 않아도 가능한 일이다. 그렇다면 왜 이렇게 번거로운 작업을 했는가? 위에서 언급한 **이 Actor의 정보를 유지해서 특정 액션을 취하는 것(목표2)**를 좀 더 용이하게 수행하기 위해서이다. Pick 시에 로직을 모두 Pick하는 Component에 작성하게 된다면 엄청나게 커플링이 발생할 것이다. 이를 방지하기위해 Delegate를 생성해서 수행하도록 하겠다. 그 예시로 Picking 중 Actor를 이동시키는 기능을 만들어보겠다.

우선 PickActorComponent를 유연하게 사용할 수 있도록 로직을 추가해야한다. Delegate를 먼저 선언하고 StartPick과 EndPick에 각각 넣어주고 로그를 PlayerContoller로 빼보겠다.

```c++
//PickActorComponent.h 에 추가

DECLARE_MULTICAST_DELEGATE_OneParam(FPickStartDelegate, AActor*);
DECLARE_MULTICAST_DELEGATE_OneParam(FPickEndDelegate, AActor*);

FPickStartDelegate OnPickStartDelegate;
FPickEndDelegate OnPickEndDelegate;
```

```c++
//PickActorComponent.cpp에 각각 delegate 넣고 로그는 주석처리
bool UPickActorComponent::StartPick(ECollisionChannel inChannel)
{
	UWorld* world = GetWorld();
	if (!IsValid(world))
	{
		UE_LOG(LogTemp, Warning, TEXT("[PickActorComponent]StartPick Fail! World is Null."));
		return false;
	}

	APlayerController* playerController = world->GetFirstPlayerController();
	if (!IsValid(playerController))
	{
		UE_LOG(LogTemp, Warning, TEXT("[PickActorComponent]StartPick Fail! CachePlayerController is Null."));
		return false;
	}

	FHitResult outHit;
	if (playerController->GetHitResultUnderCursor(inChannel, false, outHit))
	{
		if (outHit.bBlockingHit)
		{
			PickedActor = outHit.GetActor();
			if (IsValid(PickedActor))
			{
				OnPickStartDelegate.Broadcast(PickedActor);
				//UE_LOG(LogTemp, Warning, TEXT("[PickActorComponent]PickStart! Picked Object Name : %s."), *PickedActor->GetName());
				return true;
			}
		}
	}

	return false;
}

void UPickActorComponent::EndPick()
{
	if (IsValid(PickedActor))
	{
		//UE_LOG(LogTemp, Warning, TEXT("[PickActorComponent]PickEnd! Picked Object Name : %s."), *PickedActor->GetName());
		OnPickStartDelegate.Broadcast(PickedActor);
		PickedActor = nullptr;
	}
}
```

```c++
//APickingPlayerController.h에 콜백 선언
UFUNCTION()
	void OnPickStart(AActor* inPickedObject);

UFUNCTION()
	void OnPickEnd(AActor* inPickedObject);
```

```c++
//APickingPlayerController.cpp에 delegate연결하고 로그찍는 로직 추가
void APickingPlayerController::BeginPlay()
{
	Super::BeginPlay();

	bShowMouseCursor = true;
	PickActorComponent->OnPickStartDelegate.AddUObject(this, &APickingPlayerController::OnPickStart);
	PickActorComponent->OnPickEndDelegate.AddUObject(this, &APickingPlayerController::OnPickEnd);
}

void APickingPlayerController::OnPickStart(AActor* inPickedObject)
{
	UE_LOG(LogTemp, Warning, TEXT("[Picking]PickStart! Picked Object Name : %s."), *inPickedObject->GetName());
}

void APickingPlayerController::OnPickEnd(AActor* inPickedObject)
{
	UE_LOG(LogTemp, Warning, TEXT("[Picking]PickEnd! Picked Object Name : %s."), *inPickedObject->GetName());
}
```

컴파일 후 실행하면 같은 결과를 얻을 수 있다. 위에서 작업한 것처럼 원하는 로직을 얼마든지 외부에서 연결할 수 있을 것이고 PickActorComponent는 Pick외의 다른 로직과 섞이지 않을 것이다.

위 기능을 좀 더 활용해서 이동을 해보겠다. 이동을 하기위해서는 Pick한 상태에서 Pick한 Actor를 마우스 포인터의 위치를 따라와야 할 것이다. 이를 위해 코드를 먼저 수정해주자! Tick Delegate를 추가하고 TickComponent를 구현하고 PlayerController에서 연결해주자

```c++
//PickActorComponent.h 에 Delegate 선언
DECLARE_MULTICAST_DELEGATE_OneParam(FPickTickDelegate, AActor*);

virtual void TickComponent(float DeltaTime, ELevelTick TickType, FActorComponentTickFunction* ThisTickFunction) override;

FPickTickDelegate OnPickTickDelegate;
```

```c++
//PickActorComponent.h 에 tick 활성화 후 TickCompoennt 오버라이딩
UPickActorComponent::UPickActorComponent()
{
	PrimaryComponentTick.bCanEverTick = true;
}

void UPickActorComponent::TickComponent(float DeltaTime, ELevelTick TickType, FActorComponentTickFunction* ThisTickFunction)
{
	Super::TickComponent(DeltaTime, TickType, ThisTickFunction);

	if (IsValid(PickedActor))
	{
		OnPickTickDelegate.Broadcast(PickedActor);
	}
}

```

```c++
//PickingPlayerController.h
UFUNCTION()
	void OnPicking(AActor* inPickedObject);
```

```c++
//PickingPlayerController.cpp
//우선 GetHitResultUnderCursor에 파라미터를 ECC_GameTraceChannel2로 넣는다
void APickingPlayerController::BeginPlay()
{
	Super::BeginPlay();

	bShowMouseCursor = true;
	PickActorComponent->OnPickStartDelegate.AddUObject(this, &APickingPlayerController::OnPickStart);
	PickActorComponent->OnPickEndDelegate.AddUObject(this, &APickingPlayerController::OnPickEnd);
	PickActorComponent->OnPickTickDelegate.AddUObject(this, &APickingPlayerController::OnPicking);
}
	
void APickingPlayerController::OnPicking(AActor* inPickedObject)
{
	FHitResult outHit;
	if (GetHitResultUnderCursor(ECC_GameTraceChannel2, false, outHit))
	{
		inPickedObject->SetActorLocation(outHit.ImpactPoint);
	}
}

```

위에서 OnPicking함수에 GetHitResultUnderCursor 파라미터로 ECC_GameTraceChannel2로 넣어주었는데 Pick 시 바닥이 될 Actor의 채널이다. 이를 추가해주자.

!['바닥 TraceChannel 추가'](/assets/resource/2020-11-05-actor-picking-component/pickpanel.png)

Level상에 있는 Floor 액터를 아웃라이너에서 찾아서 위에서 만든 Trace Channel을 켜준다. 

!['Floor 콜리젼 변경'](/assets/resource/2020-11-05-actor-picking-component/floorcollision.png)

그리고 추가했던 큐브는 움직여야하기 떄문에 무버블로 변경해준다.

!['무버블'](/assets/resource/2020-11-05-actor-picking-component/movable.png)

이제 큐브를 잡고 이동할 수 있게 되었다.

!['무버블'](/assets/resource/2020-11-05-actor-picking-component/pickingmove.gif)

만약 저 큐브가 캐릭터였다면 OnPickStart에서 애니메이션을 다르게 플레이하거나 메테리얼 색을 변경할수도 있을것이며 OnPicking에서 위치를 조절할 수도 있을 것이다. 만약 UI가 있다면 PlayerController에 선언된 PickActorComponent에 선택된 Actor의 이름을 출력하는 기능도 PickActorComponent를 수정하지 않고 추가할 수 있을 것이다.


[GetHitResultUnderCursor]: https://docs.unrealengine.com/en-US/API/Runtime/Engine/GameFramework/APlayerController/GetHitResultUnderCursor/index.html