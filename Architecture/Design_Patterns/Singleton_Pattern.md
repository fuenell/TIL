# 싱글톤 패턴 (Singleton Pattern)
싱글톤 패턴은 해당 클래스의 인스턴스가 하나만 만들어지고  
어디서든지 그 인스턴스에 접근할 수 있도록 하기 위한 패턴이다

## 고전적 싱글톤 패턴
`GetInstance()` 함수를 통해서만 객체를 생성 및 반환을 해 인스턴스를 하나로 유지한다
``` C#
class ClassicSingleton
{
    private static ClassicSingleton instance;

    private ClassicSingleton() { }    // 생성자를 private으로 선언해 외부에서 인스턴스를 생성할 수 없게 한다

    public static ClassicSingleton GetInstance()
    {
        if (instance == null)
            instance = new ClassicSingleton();
        return instance;
    }
}
```
## 멀티스레딩 문제
위에서 보여준 싱글톤 패턴은 그냥 봤을 때는 문제 없이 사용할 수 있을 것 같지만 몇가지 문제점이 있다  
대표적으로 멀티스레드를 사용 중이라면 동시에 `GetInstance()`를 호출해 인스턴스가 2개 생길 수 있다  
이를 해결하려면 GetInstance()를 동시화 시키는 방법이 있다  
C#에서는 lock 키워드로 구역을 동기화시킬 수 있다
``` C#
class SyncSingleton
{
    private static SyncSingleton instance;
    private static object lockObject = new object();

    private SyncSingleton() { }

    public static SyncSingleton GetInstance()
    {
        lock (lockObject)   // 이 구역이 동기화 됨
        {
            if (instance == null)
                instance = new SyncSingleton();
            return instance;
        }
    }
}
```
또는 인스턴스를 프로그램이 시작할 때 부터 만드는 방법이 있다
``` C#
class InitSingleton
{
    private static InitSingleton instance = new InitSingleton();

    private InitSingleton() { }

    public static InitSingleton GetInstance()
    {
        return instance;
    }
}
```
