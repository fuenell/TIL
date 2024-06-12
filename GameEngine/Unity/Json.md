# Json
Unity에서는 기본적으로 `JsonUtility`클래스로 Json 변환을 지원한다.

## 사용법
Json 변환 시 `public` 또는 `[SerializeField]`인 변수만 Json으로 변환된다.
```C#
public class Test : MonoBehaviour
{
    void Start()
    {
        MyData myData1 = new MyData();
        string jsonString = JsonUtility.ToJson(myData1);
        MyData myData2 = JsonUtility.FromJson<MyData>(jsonString);
    }
}

public class MyData
{
    [SerializeField] int num;
    public string str;
    public List<int> list;  // Json 변환이 가능한 제네릭 인수만 가능
    public MyData2 m2;  // [Serializable] 속성을 받은 클래스는 변환 가능 (최대 깊이 10)
}

[Serializable]
public class MyData2
{
    public int num2;
}
```

## 기타
`Dictionary<K, V>` 자료형은 `JsonUtility`로 변환이 불가능하다.

만약 Dictionary 자료형을 Json으로 변환하려면 Key-Value 쌍을 가진 List로 변환한 뒤 Json으로 변환할 수 있으며, 불러올 때도 List를 다시 Dictionary로 변환하는 작업을 거쳐야 한다.

또 다른 방법으로는 `com.unity.nuget.newtonsoft-json`과 같은 Dictionary 변환을 지원하는 외부 라이브러리를 설치하여 사용하는 것이다.