# Find Asset by Label [#API](https://docs.unity3d.com/ScriptReference/AssetDatabase.FindAssets.html)
라벨 설정법과 라벨로 에셋 데이터베이스에서 에셋의 경로를 구하는 함수 설명

## 라벨 설정
에셋을 선택 후 우측 하단의 라벨 아이콘을 클릭하면 라벨을 설정할 수 있다  
라벨명을 직접 입력하면 목록에 없는 라벨도 자동으로 생성된다   
![set_label](https://user-images.githubusercontent.com/37904040/124862348-2d4b8080-dff0-11eb-841e-fa3643448749.jpg)  

## 라벨 검색
``` c#
string findLabel = "Sky";   // 검색할 라벨
string[] guids = AssetDatabase.FindAssets("l:" + findLabel, null);   // [l:] 필터로 해당 라벨이 있는 모든 에셋의 GUID를 반환한다
foreach (string guid in guids)
{
    string path = AssetDatabase.GUIDToAssetPath(guid);      // GUID로 에셋의 경로를 구한다
    T asset = AssetDatabase.LoadAssetAtPath<T>(path);       // AssetDatabase에서 에셋을 로드한다 (T는 UnityEngine.Object 상속 객체)
}
```
