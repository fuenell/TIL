# 옵저버 패턴 (Observer Pattern)
옵저버 패턴은 한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들에게 정보를 알려주는 패턴이다 (일대다 의존성이다)
- 느슨한 결합(서로 상호작용을 하지만 서로에 대해 모르는 상태)을 사용해 객체 상호의존성을 줄일 수 있다

## Push 형태 옵저버 패턴
모든 옵저버들에게 정해진 데이터 형식의 데이터를 전송한다  
- 보낼 데이터 형식이 추가되면 모든 Update 함수를 고쳐야한다
- 필요없는 데이터를 수신하는 옵저버가 생길 수 있다

![옵저버_Push](https://github.com/fuenell/TIL/assets/37904040/a3b884dd-5562-41df-af72-7f348b6e3a08)

## Pull 형태 옵저버 패턴
각 옵저버들에게 값이 변경되었다는 것을 알려주고  
각 옵저버들은 주제 객체에서 원하는 정보를 스스로 가져간다  
- 위의 단점을 커버할 수 있다
- 옵저버가 옵저블 클래스를 알고있어야 한다 (상호의존성 증가)

![옵저버_Pull](https://github.com/fuenell/TIL/assets/37904040/10adf428-761b-463e-a31a-2d1a8e2fb2c5)

## Unity 옵저버 패턴
``` C#
public abstract class Observer : MonoBehaviour
{
    public abstract void Notify();
}

public abstract class Subject : MonoBehaviour
{
    private readonly List<Observer> _observers = new List<Observer>();

    protected void Attach(Observer observer)
    {
        _observers.Add(observer);
    }

    protected void Detach(Observer observer)
    {
        _observers.Remove(observer);
    }

    protected void NotifyObservers()
    {
        foreach (Observer observer in _observers)
        {
            observer.Notify();
        }
    }
}
```