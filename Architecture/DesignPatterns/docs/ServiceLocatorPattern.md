# 서비스 로케이터 패턴 (Service Locator Pattern)
환경 별로 서로 다른 서비스를 사용해야할 경우 이 패턴을 쓸 수 있다.

예를 들어 안드로이드와 ios 환경으로 빌드를 하는 경우이다.

각 플랫폼에 맞는 서비스들을 묶어서 딕셔너리로 저장해 반환하는 방식이다.

``` C#
public class LoginManager : MonoBehaviour
{
    void Start()
    {
        RegisterPlatformLogin();
    }

    private void RegisterPlatformLogin()
    {
#if UNITY_ANDROID
        ServiceLocator.RegisterService<ILoginService>(new AndroidLoginService());
        ServiceLocator.RegisterService<ILogService>(new AndroidLogService());
#elif UNITY_IOS
        ServiceLocator.RegisterService<ILoginService>(new IOSLoginService());
        ServiceLocator.RegisterService<ILogService>(new IOSLogService());
#else
        Debug.LogWarning("Unsupported platform");
#endif
    }

    void DoLogin()
    {
        ILoginService loginService = ServiceLocator.GetService<ILoginService>();
        loginService.Login();
        
        ILogService logService = ServiceLocator.GetService<ILogService>();
        logService.Log("로그인 성공");
    }
}
```

``` C#
public static class ServiceLocator
{
    private static Dictionary<Type, object> _services = new Dictionary<Type, object>();

    public static void RegisterService<T>(T service)
    {
        var type = typeof(T);
        if (!_services.ContainsKey(type))
        {
            _services.Add(type, service);
        }
    }

    public static T GetService<T>()
    {
        var type = typeof(T);
        if (_services.TryGetValue(type, out var service))
        {
            return (T)service;
        }

        throw new Exception($"Service of type {type} not found");
    }
}
```