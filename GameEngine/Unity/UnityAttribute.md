# Unity Attribute
유니티에서 자주 사용하는 속성 정리

## Header
인스펙터에서 변수 위에 헤더를 추가하는 속성이다.
``` C#
[Header("Int variable")]
public int a;
```

## Space
변수 위에 빈공간을 둔다.
``` C#
public int a;

[Space(100)]
public int b;
```

## Tooltip

``` C#
[Tooltip("변수 a에 대한 설명")]
public int a;
```

## Range

``` C#
[Range(0, 10)]
public int a;
```

## TextArea

``` C#
[TextArea]
public string a;
```

## Header

``` C#
[Multiline]
public string a;
```

## SerializeField

``` C#
[SerializeField]
private int a;
```

## HideInInspector

``` C#
[HideInInspector]
public int a;
```

## ContextMenuItem
변수 우클릭 후 함수 실행
``` C#
[ContextMenuItem("Print A", "PrintA")]
public int a;

void PrintA()
{
    print(a);
}
```

## ContextMenu
스크립트 우클릭 후 함수 실행
``` C#
[ContextMenu("Print Hello, World!")]
void PrintHelloWorld()
{
    print("Hello, World!");
}
```

## HelpURL
? 아이콘을 눌렀을 때 열리는 사이트 설정
``` C#
[HelpURL("https://www.youtube.com/")]
public class AttributeTestScript : MonoBehaviour {}
```

## RequireComponent
필수 컴포넌트를 지정할 수 있다.
``` C#
[RequireComponent(typeof(Rigidbody), typeof(BoxCollider), typeof(MeshRenderer))]
[RequireComponent(typeof(AudioSource), typeof(AudioListener))]
public class AttributeTestScript : MonoBehaviour
{

}
```

## SelectionBase
해당 속성을 가진 오브젝트의 자식 오브젝트를 선택했을 때 부모 오브젝트가 먼저 선택되는 속성이다.
``` C#
[SelectionBase]
public class AttributeTestScript : MonoBehaviour
{
}
```

## AddComponentMenu
해당 속성을 가진 오브젝트의 자식 오브젝트를 선택했을 때 부모 오브젝트가 먼저 선택되는 속성이다.
``` C#
[AddComponentMenu("Custom/New Component")]
public class AttributeTestScript : MonoBehaviour
{

}
```

## ExecuteInEditMode
해당 속성을 가진 오브젝트의 자식 오브젝트를 선택했을 때 부모 오브젝트가 먼저 선택되는 속성이다.
``` C#
[ExecuteInEditMode]
public class AttributeTestScript : MonoBehaviour
{
    private void Update()
    {
        print("a");
    }
}

```

## CreateAssetMenu

``` C#
[CreateAssetMenu(fileName = "SoundList", menuName = "ScriptableObject/SoundList", order = int.MaxValue)]
```
