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

## Unity의 싱글톤 패턴
유니티에 MonoBehavior를 상속받는 클래스와 오브젝트를 싱글톤으로 유지하려면 아래와 같은 방법이 있다.
``` C#
class UnitySingleton : MonoBehaviour
{
    private static UnitySingleton _instance;
    public static UnitySingleton Instance
    {
        get
        {
            if(_instance == null)
            {
                _instance = FindObjectOfType<UnitySingleton>();

                if (_instance == null)
                {
                    GameObject obj = new GameObject(nameof(UnitySingleton));
                    _instance = obj.AddComponent<UnitySingleton>();
                }
            }
            return _instance;
        }
    }

    private void Awake()
    {
        if(_instance == null)
        {
            _instance = this;
            DontDestroyOnLoad(gameObject);
        }
        else if(_instance != this)
        {
            Destroy(this);  // Destroy(gameObject) 가 더 깔끔하지만 하나의 오브젝트에 2개의 싱글톤 클래스가 있으면 통째로 사라질 수 있음
            return;
        }
    }
}
```

약간의 문제점은 아래의 코드 호출 시 new GameObject()가 호출되는 시점에서 Awake가 실행된다.  
Awake에서 _instance가 할당되기 때문에 2번째 줄은 사실상 없어도 문제는 없다. (코드 가독성 때문에 남겨둠)
``` C#
GameObject obj = new GameObject(nameof(UnitySingleton));
_instance = obj.AddComponent<UnitySingleton>();
```
