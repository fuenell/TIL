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
    public int a;
    public string b;
}
```
![create_sobj](https://user-images.githubusercontent.com/37904040/123229342-2a8d5d80-d511-11eb-9281-a86a9df25357.jpg)

이후 생성된 에셋을 수정해 값을 저장할 수 있다

![edit_sobj](https://user-images.githubusercontent.com/37904040/123229347-2bbe8a80-d511-11eb-8f97-decf4f6eea89.jpg)

## 사용법
생성된 ScriptableObejct는 SerializeField를 통해 참조할 수 있다
