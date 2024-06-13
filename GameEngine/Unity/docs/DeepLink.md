## DeepLink [#Manual](https://docs.unity3d.com/kr/2020.3/Manual/enabling-deep-linking.html)
딥 링크는 애플리케이션의 특정 콘텐츠에 직접 연결되는 링크다.

## Android URL 스킴
`Project Settings > Player > Android > Publishing Setting > Build > Custom Main Manifest 체크`  
이후 `Assets/Plugins/Android/AndroidManifest.xml` 경로로 생성된 xml파일에 아래 xml을 덮어쓴다.
``` xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" xmlns:tools="http://schemas.android.com/tools">
  <application>
    <activity android:name="com.unity3d.player.UnityPlayerActivity" android:theme="@style/UnityThemeSelector" >
      <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
      </intent-filter>
      <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="unitydl" android:host="mylink" />
      </intent-filter>
    </activity>
  </application>
</manifest>
```
`<data android:scheme="unitydl" android:host="mylink" />` 이 줄에서 `scheme(unitydl)`는 URL 체계, `host(mylink)`는 앱 주소이다.  
위 Manifest가 등록된 앱이 설치되어 있으면, `unitydl://mylink`의 링크를 열었을 때 해당 앱이 실행된다.

## iOS URL 스킴
`Project Settings > Player > iOS > Other Settings > Configuration > Supported URL schemes 필드에 URL 체계 입력` (예: unitydl)  
~~설정 완료 후 `unitydl://`로 앱을 실행할 수 있다.~~ (확인 필요)

## 값 전달
`Application.deepLinkActivated`로 딥 링크를 통한 앱 실행 이벤트를 감지할 수 있다.  
또한 주소 뒤에 ?와 값을 붙여 값을 전달할 수 있다.
``` C#
public class ProcessDeepLinkMngr : MonoBehaviour
{
    public static ProcessDeepLinkMngr Instance { get; private set; }
    public string deeplinkURL;
    private void Awake()
    {
        if (Instance == null)
        {
            Instance = this;                
            Application.deepLinkActivated += onDeepLinkActivated;
            if (!String.IsNullOrEmpty(Application.absoluteURL))
            {
                onDeepLinkActivated(Application.absoluteURL);
            }
            else deeplinkURL = "[none]";
            DontDestroyOnLoad(gameObject);
        }
        else
        {
            Destroy(gameObject);
        }
    }
 
    private void onDeepLinkActivated(string url)
    {
        deeplinkURL = url;
        string parameter = url.Split('?')[1];
    }
}
```

## 2세대 딥 링크
URL 스킴에는 치명적인 2가지 단점이 존재한다.
1. 같은 URL 스킴을 사용하는 앱이 2개 이상일 경우 정상적으로 작동하지 않는다.
2. 만약 해당 앱이 설치되어 있지 않다면 딥 링크는 작동하지 않는다.  

때문에 이를 방지하고자 앱이 설치되어 있지 않으면 스토어로 이동하는 딥 링크가 개발되었다.  
(iOS에서는 유니버설 링크, Android에서는 앱 링크)  
구체적인 구현 방법은 기술하지 않는다.
