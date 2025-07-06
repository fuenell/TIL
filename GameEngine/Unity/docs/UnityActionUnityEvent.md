# UnityAction, UnityEvent
유니티 C#에서 사용 가능한 함수 참조형은 5가지가 있다.
- 델리게이트(delegate)
- 이벤트(event)
- 액션(Action)
- 유니티 액션(UnityAction)
- 유니티 이벤트(UnityEvent)

각 특징만 잘 이해하고 있다면 어떤 것을 사용해도 상관 없다.

## 델리게이트(delegate)
**델리게이트**는 C#에서 제공하는 타입 안전한 함수 포인터이다. 특정 메서드를 참조하고, 이후에 해당 메서드를 실행할 수 있게 해준다. 여러 메서드를 하나의 델리게이트에 추가하여 호출할 수 있으며, 이벤트 기반 프로그래밍에서 주로 사용된다.

- **주요 특징**: 
  - enum 처럼 직접 형태를 선언해서 사용해야 한다. 
  - 반환형을 설정할 수 있다.

```csharp
public delegate void MyDelegate(string message);

public class Test
{
    public MyDelegate myDelegate;

    public void Execute()
    {
        // 델리게이트에 외부 메서드 추가
        myDelegate += PrintMessage;
        myDelegate += PrintAnotherMessage;

        // ShowMessage를 호출하면서 등록된 모든 메서드 호출
        myDelegate("Test");
    }

    // 델리게이트에 할당할 메서드
    public void PrintMessage(string message)
    {
        Console.WriteLine("PrintMessage: " + message);
    }

    public void PrintAnotherMessage(string message)
    {
        Console.WriteLine("PrintAnotherMessage: " + message);
    }
}
```

## 이벤트 (event 키워드)
**이벤트**는 델리게이트를 외부에서 직접 호출하지 못하게 만드는 C#의 키워드입니다. 이벤트를 정의한 클래스 내에서만 해당 델리게이트를 호출할 수 있게 제한합니다.

- **주요 특징**:
  - 내부에서만 호출 가능 (event 키워드)
  - 외부에서는 +=, -=로 등록/제거만 가능
  - 안전한 이벤트 설계를 위해 사용
  - delegate, Action, UnityAction 등 delegate 기반 타입에만 사용 가능

```csharp
public class Test
{
    public event MyDelegate myEvent;

    public void TriggerEvent()
    {
        myEvent += PrintMessage;

        myEvent?.Invoke("Test");
    }
}
```

## 액션(Action)
**Action**은 C#의 내장 델리게이트로, 반환값이 없는 함수에 사용된다. 델리게이트를 정의하지 않고도 쉽게 사용할 수 있으며, 매개변수 타입만 정의하면 된다. `Action<T>` 형태로 사용하며, 최대 16개의 매개변수를 받을 수 있다.

- **주요 특징**:
  - 반환값이 없는 함수에 사용
  - 간편한 내장 델리게이트
  - 매개변수 타입 지정 가능

```csharp
Action<string> myAction = (message) => Console.WriteLine(message);
myAction("Hello, Action!");
```

## UnityAction
**UnityAction**은 Unity에서 제공하는 `UnityEngine.Events` 네임스페이스 내의 델리게이트 타입으로, Unity의 이벤트 시스템에 특화되어 있다. `Action`과 비슷하지만, Unity의 이벤트 시스템과 긴밀하게 연결되어 있어 Unity의 컴포넌트와 쉽게 연동된다.

- **주요 특징**:
  - Unity의 이벤트 시스템과 연동
  - 인스펙터에서 이벤트를 설정하고 관리할 수 있음
  - `UnityEvent`와 함께 사용되어 메서드 참조 및 호출 관리

```csharp
UnityAction myUnityAction = () => Debug.Log("UnityAction called!");
myUnityAction.Invoke();
```

## UnityEvent
**UnityEvent**는 Unity에서 제공하는 커스텀 이벤트 시스템으로, Unity의 인스펙터에서 이벤트를 시각적으로 관리할 수 있다. 이를 통해 코드 작성 없이도 인스펙터에서 함수를 등록하고 이벤트 발생 시 자동으로 호출할 수 있는 방식으로 설정 가능하다.

- **주요 특징**:
  - 인스펙터에서 이벤트 등록 가능
  - 비개발자도 쉽게 이벤트 설정 및 관리 가능

```csharp
public UnityEvent myUnityEvent;

public void TriggerUnityEvent()
{
    myUnityEvent.AddListener(DoSomething);
    myUnityEvent?.Invoke();
}
```

UnityEvent는 유니티의 게임 오브젝트, UI 요소 등과 연결하여 이벤트를 처리하는 데 매우 유용하며, 인스펙터에서 간편하게 이벤트 핸들러를 설정할 수 있어 개발과 디자이너 협업에 적합한 시스템이다.
