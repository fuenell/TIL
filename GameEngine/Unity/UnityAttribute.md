# Unity Attribute
유니티에서 자주 사용하는 특성 정리
기본적으로 `MonoBehaviour`를 상속받는 스크립트에서 사용한다.

- [Header](#header)
- [Space](#space)
- [Tooltip](#tooltip)
- [TextArea](#textarea)
- [Multiline](#multiline)
- [SerializeField](#serializefield)
- [HideInInspector](#hideininspector)
- [ContextMenuItem](#contextmenuitem)
- [ContextMenu](#contextmenu)
- [HelpURL](#helpurl)
- [Header](#header)

## Header
인스펙터에서 변수 위에 헤더를 추가하는 특성이다.
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

## Multiline

``` C#
[Multiline]
public string a;
```

## SerializeField
변수에 사용하는 특성으로, 인스펙터에서 변수를 숨길 수 있다.
``` C#
[SerializeField]
private int a;
```

## HideInInspector
변수에 사용하는 특성으로, 인스펙터에서 변수를 숨길 수 있다.
``` C#
[HideInInspector]
public int a;
```

## ContextMenuItem
변수에 사용하는 특성으로, 인스펙터에서 변수를 우클릭 후 지정된 메소드를 실행 시킬 수 있다.
``` C#
[ContextMenuItem("Print A", "PrintA")]
public int a;

void PrintA()
{
    print(a);
}
```

## ContextMenu
클래스에 사용하는 특성으로, 스크립트 우상단의 `⋮` 버튼을 눌러 함수를 실행할 수 있게 된다.
``` C#
[ContextMenu("Print Hello, World!")]
void PrintHelloWorld()
{
    print("Hello, World!");
}
```

## HelpURL
클래스에 사용하는 특성으로, 컴포넌트 우상단의 `?` 버튼을 눌렀을 때 연결되는 사이트를 설정할 수 있다.
``` C#
[HelpURL("https://www.youtube.com/")]
public class AttributeTestScript : MonoBehaviour {}
```

## RequireComponent
클래스에 사용하는 특성으로, 해당 스크립트가 있는 오브젝트에 필수 컴포넌트를 지정할 수 있다.
한줄에 3개의 컴포넌트를 설정할 수 있기 때문에, 4개 이상의 컴포넌트를 원한다면 여러줄에 나눠 작성해야 한다.
``` C#
[RequireComponent(typeof(Rigidbody), typeof(BoxCollider)]
public class AttributeTestScript : MonoBehaviour
{

}
```

## SelectionBase
클래스에 사용하는 특성으로, 씬에서 해당 특성을 가진 오브젝트의 자식 오브젝트를 선택했을 때 부모 오브젝트가 먼저 선택된다.
``` C#
[SelectionBase]
public class AttributeTestScript : MonoBehaviour
{
}
```

## AddComponentMenu
클래스에 사용하는 특성으로, 해당 스크립트를 컴포넌트 메뉴에 추가한다.
``` C#
[AddComponentMenu("Custom/Test")]
public class AttributeTestScript : MonoBehaviour
{

}
```

## ExecuteInEditMode
클래스에 사용하는 특성으로, 해당 스크립트는 씬을 실행하지 않아도 에디터에서 실행된다.
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
ScriptableObject에 사용하는 특성으로 에셋 생성 메뉴를 추가한다.
``` C#
[CreateAssetMenu(fileName = "Test", menuName = "ScriptableObject/Test", order = 0)]
public class AttributeTestScript : ScriptableObject
{

}
```
![image](https://user-images.githubusercontent.com/37904040/216889746-fcaed652-deb8-4195-b44c-5533d6c418cc.png)
