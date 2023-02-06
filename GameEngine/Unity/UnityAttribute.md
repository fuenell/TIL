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
- [RequireComponent](#requirecomponent)
- [SelectionBase](#selectionbase)
- [AddComponentMenu](#addcomponentmenu)
- [ExecuteInEditMode](#executeineditmode)
- [CreateAssetMenu](#createassetmenu)

## Header
변수에 사용하는 특성으로, 변수 상단에 제목을 추가할 수 있다.
``` C#
[Header("Int variable")]
public int a;
```
![레이어 10](https://user-images.githubusercontent.com/37904040/216907231-bd540e2f-97c8-4ab7-8a1e-ed9fc7c6c42a.png)


## Space
변수에 사용하는 특성으로, 변수의 상단여백 크기를 설정할 수 있다.
``` C#
public int a;

[Space(100)]
public int b;
```
![레이어 9](https://user-images.githubusercontent.com/37904040/216907247-ae5d31f8-cfee-4e55-8e2d-8a809bb36126.png)


## Tooltip
변수에 사용하는 특성으로, 변수에 마우스를 올렸을 때 도움말이 나타나도록 설정할 수 있다.
``` C#
[Tooltip("변수 a에 대한 설명")]
public int a;
```
![레이어 8](https://user-images.githubusercontent.com/37904040/216907894-9d093b45-5f03-4a98-a7eb-67c7e1f47b21.png)


## Range
int, float 변수에 사용하는 특성으로, 값의 범위를 지정할 수 있다. (슬라이더 형식으로 조절도 가능하다)
``` C#
[Range(0, 10)]
public int a;
```
![레이어 7](https://user-images.githubusercontent.com/37904040/216907280-92e91aeb-77be-496c-97db-f5cf50bc0b32.png)


## TextArea
string 변수에 사용하는 특성으로, 여러 줄을 작성할 수 있다.
``` C#
[TextArea]
public string a;
```
![레이어 6](https://user-images.githubusercontent.com/37904040/216907290-35fd5659-8c15-4a24-91ce-55a7b447e9ef.png)


## Multiline
string 변수에 사용하는 특성으로, 여러 줄을 작성할 수 있다.
``` C#
[Multiline]
public string a;
```
![레이어 5](https://user-images.githubusercontent.com/37904040/216907307-a173c674-7dde-4273-a6f7-f9214324817d.png)


## SerializeField
변수에 사용하는 특성으로, 인스펙터에서 private 변수를 표시할 수 있다.
``` C#
[SerializeField]
private int a;
```


## HideInInspector
변수에 사용하는 특성으로, 인스펙터에서 public 변수를 숨길 수 있다.
``` C#
[HideInInspector]
public int a;
```


## ContextMenuItem
변수에 사용하는 특성으로, 변수를 우클릭 후 지정된 메소드를 실행 시킬 수 있다.
``` C#
[ContextMenuItem("Print A", "PrintA")]
public int a;

void PrintA()
{
    print(a);
}
```
![레이어 4](https://user-images.githubusercontent.com/37904040/216907325-e8cc4cf0-85d3-4da6-b5fa-fce0162bb7e4.png)


## ContextMenu
클래스에 사용하는 특성으로, 스크립트 우상단의 `⋮` 버튼을 눌러 함수를 실행할 수 있게 된다.
``` C#
[ContextMenu("Print Hello, World!")]
void PrintHelloWorld()
{
    print("Hello, World!");
}
```
![레이어 3](https://user-images.githubusercontent.com/37904040/216907341-6d3ac403-2a77-4c5f-be22-5f239a9a28a8.png)


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
![레이어 1](https://user-images.githubusercontent.com/37904040/216907401-f3f80ad8-c636-4ed9-95cd-3845cb460a79.png)


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
