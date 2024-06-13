# ScriptableObejct [#Manual](https://docs.unity3d.com/kr/2020.3/Manual/class-ScriptableObject.html)
ScriptableObejct는 클래스 인스턴스와는 별개로 데이터를 저장할 수 있는 컨테이너다

## 생성법
ScriptableObject를 상속받아 선언할 수 있다
그러나 선언만으로는 사용할 수 없고 CreateAssetMenu 속성을 통해 생성 메뉴를 추가해 Asset을 생성해야한다
``` c#
using UnityEngine;

[CreateAssetMenu(fileName = "Value", menuName = "Scriptable Object/Value", order = int.MaxValue)]
public class Value : ScriptableObject
{
    public string title;
    public int[] keys;
}
```
![create_sobj](https://user-images.githubusercontent.com/37904040/123229342-2a8d5d80-d511-11eb-9281-a86a9df25357.jpg)

이후 생성된 에셋을 수정해 값을 저장할 수 있다

![edit_sobj](https://user-images.githubusercontent.com/37904040/123229347-2bbe8a80-d511-11eb-8f97-decf4f6eea89.jpg)

## 사용법
생성된 ScriptableObejct는 pulibc이나 SerializeField를 통해 참조할 수 있다
``` c#
public class A : MonoBehaviour
{
    public Value value;

    void Start()
    {
        print(value.title);
    }
}
```
![public_sobj](https://user-images.githubusercontent.com/37904040/123354906-8acae080-d59f-11eb-9d42-491f7b103251.jpg)

## AssetDatabase 참조
위 사용법이 간단하고 쉽지만 MonoBehaviour를 상속받지 않는 클래스의 경우에는 같은 방법으로 참조가 불가능하다  
그런 상황에는 AssetDatabase 통해 객체를 얻어 사용할 수 있다  
또한 클래스에서 에셋을 수정하면 값이 변경되었다는 것을 에디터가 인지하지 못할 수 있기 때문에, 아래 코드를 통해 변경사항을 저장 가능하다
``` c#
Value value = AssetDatabase.LoadAssetAtPath<Value>("Assets/Value.asset");
value.title = "NewTitle";

EditorUtility.SetDirty(value);      // 해당 객체가 수정되었다는 것을 에디터에게 알려준다
AssetDatabase.SaveAssets();         // 생성, 수정된 에셋이 있다면 저장한다
```
