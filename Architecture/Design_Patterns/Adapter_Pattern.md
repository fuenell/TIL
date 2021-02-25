# 어댑터 패턴 (Adapter Pattern)
어댑터 패턴은 한 인터페이스를 다른 인터페이스로 변환하는 패턴이다  
데코레이터 패턴과 유사하게 클래스를 다른 클래스로 감싸는 방식을 사용한다

## 예제
피처폰 인터페이스를 스마트폰 인터페이스로 변환하는 예제
``` C#
static void Main(string[] args)
{
    ISmartPhone galaxy = new GalaxyPhone();
    IFeaturePhone lollipop = new LollipopPhone();

    ISmartPhone s_lollipop = new FeaturePhoneAdapter(lollipop);     // 피처폰을 스마트폰 인터페이스로 변환

    galaxy.MakeCall("010-0000-0000");
    s_lollipop.MakeCall("010-0000-0000");

    galaxy.WatchYoutube();
    s_lollipop.WatchYoutube();    // 불가능하다는 메시지 출력
}
```
``` C#
public interface ISmartPhone
{
    public void MakeCall(string phoneNumber);
    public void SendMessage(string phoneNumber, string message);
    public void WatchYoutube();
}

class GalaxyPhone : ISmartPhone
{
    public void MakeCall(string phoneNumber)
    {
        Console.WriteLine("[" + phoneNumber + "]으로 전화를 겁니다");
    }
    public void SendMessage(string phoneNumber, string message)
    {
        Console.WriteLine("[" + phoneNumber + "]으로 \"" + message + "\"라는 메시지를 전송합니다");
    }
    public void WatchYoutube()
    {
        Console.WriteLine("유튜브 어플을 통해 영상을 시청합니다");
    }
}

public interface IFeaturePhone
{
    public void MakeCall(string phoneNumber);
    public void SendMessage(string phoneNumber, string message);
}

class LollipopPhone : IFeaturePhone
{
    public void MakeCall(string phoneNumber)
    {
        Console.WriteLine("[" + phoneNumber + "]으로 전화를 겁니다");
    }
    public void SendMessage(string phoneNumber, string message)
    {
        Console.WriteLine("[" + phoneNumber + "]으로 \"" + message + "\"라는 메시지를 전송합니다");
    }
}
```

``` C#
public class FeaturePhoneAdapter : ISmartPhone
{
    IFeaturePhone featurePhone;

    public FeaturePhoneAdapter(IFeaturePhone featurePhone)
    {
        this.featurePhone = featurePhone;
    }

    public void MakeCall(string phoneNumber)
    {
        featurePhone.MakeCall(phoneNumber);
    }

    public void SendMessage(string phoneNumber, string message)
    {
        featurePhone.SendMessage(phoneNumber, message);
    }

    public void WatchYoutube()
    {
        Console.WriteLine("피처폰에서는 유튜브를 시청할 수 없습니다");
    }
}
```
