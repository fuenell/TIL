# 스테이트 패턴 (State Pattern)
상태를 기반으로 하는 행동을 캡슐화하고 행동을 현재 상태한테 위임하는 패턴

## 적용 상황

스테이트 패턴은 한번에 한가지 상태만 가질 수 있고, 같은 이벤트가 발생하더라도 상태에 따라 다른 행동을 수행할 때 사용할 수 있다

ex) TV 리모컨의 전원버튼을 눌렀을 때, TV의 상태가 ON이라면 OFF로 변하고 OFF라면 ON으로 변한다

![TV](https://user-images.githubusercontent.com/37904040/123536569-ffe81280-d765-11eb-9b1d-995720f20c0f.png)

## 열거형 스테이트 [#열거형 상태 관리자 코드 생성기](https://htmlpreview.github.io/?https://github.com/fuenell/TIL/blob/main/Architecture/DesignPatterns/Sample/EnumState.html)

스테이트 패턴을 사용하지 않더라도 enum을 활용해서 상태를 저장하고, if문으로 분기해서 간단하게 코드를 작성할 수 있습니다

그러나 상태와 이벤트가 늘어난다면 if문을 계속 추가시켜주어야 할 것입니다

```c#
enum TVState { Off, On }
TVState state;

void Power()
{
    if (state == TVState.Off)
    {
        state = TVState.On;
        Console.WriteLine("TV를 켠다");
    }
    else if (state == TVState.On)
    {
        state = TVState.Off;
        Console.WriteLine("TV를 끈다");
    }
}
```

## 스테이트 패턴

모든 이벤트를 정의한 인터페이스를 작성하고 각 상태를 클래스로 선언해 해당 인터페이스를 구현하는 방식으로 코드를 작성한다

```c#
class TV
{
    private ITVState state;
    public ITVState offState;
    public ITVState onState;

    public TV()
    {
        offState = new OffState();
        onState = new OnState();
    }

    public void SetState(ITVState newState)
    {
        state = newState;
    }

    public void Power()
    {
        state.Power(this);      // 이벤트 처리를 현재 상태에게 위임한다
    }
}

interface ITVState
{
    public void Power(TV tv);
}


class OffState : ITVState
{
    public void Power(TV tv)
    {
        Console.WriteLine("TV를 켠다");
        tv.SetState(tv.onState);
    }
}

class OnState : ITVState
{
    public void Power(TV tv)
    {
        Console.WriteLine("TV를 끈다");
        tv.SetState(tv.offState);
    }
}
```
