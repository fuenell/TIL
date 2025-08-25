# Addressables
Addressables은 에셋을 그룹 단위로 묶어서 관리해주는 라이브러리다.  
이를 활용해 메모리 관리와 빌드 이후 에셋 교체가 가능하다.

## 장점

- **메모리 관리**: Addressables를 사용하면 에셋을 필요할 때만 로드하고 해제할 수 있으므로 메모리 부담을 줄일 수 있습니다.  
또한 에셋을 그룹으로 묶어서 관리할 수 있어서 의존성 문제를 해결할 수 있습니다.

- **콘텐츠 업데이트**: Addressables를 사용하면 에셋을 원격 서버에 저장하고 빌드 이후에도 교체할 수 있으므로 콘텐츠 업데이트가 용이합니다.  
또한 에셋의 주소를 통해 로드하기 때문에 코드 수정 없이 에셋을 변경할 수 있습니다.

## 단점

- **학습**: Addressables에는 다양한 개념과 용어들이 많으므로 새롭게 학습해야 하는 부분이 있습니다.  
예를 들어, 그룹 스키마, 프로파일 변수, 카탈로그 등의 개념을 이해해야 합니다.

- **디버깅 어려움**: Addressables는 비동기 로딩 방식을 사용하므로 디버깅이 어려울 수 있습니다.  
예외 처리나 오류 메시지가 명확하지 않거나, 로드 시간이 길거나, 메모리 누수가 발생하는 등의 문제가 발생할 수 있습니다.

- **성능 저하**: Addressables는 원격 서버에서 에셋을 다운로드 받아야 하는 경우가 많으므로 네트워크 상태에 따라 성능 저하가 발생할 수 있습니다.  
또한 에셋 압축과 해제 과정에서 CPU와 GPU에 부담이 가중될 수 있습니다.

아래는 사용자가 작성한 Addressables의 "특징" 부분을 더 명확하고 실용적으로 다듬은 내용입니다:

## 특징 (정리 및 주의사항)

* **씬에서 직접 참조한 에셋은 Addressables 등록 여부와 관계없이 빌드에 포함됩니다.**
  이 경우, 동일한 에셋이 두 번 저장될 수 있습니다.
  (예: 씬 직접 참조용 / Addressables 로드용 → 중복 저장)

* 따라서 **Addressables에 등록한 에셋은 반드시 `Addressables.LoadAssetAsync<T>()` 등의 API로만 로드해야 중복 포함을 방지할 수 있습니다.**
  에디터나 코드에서 직접 참조할 경우, 해당 에셋은 Addressables 외에도 기본 빌드에도 포함됩니다.

* **Addressables 프리팹이 참조하고 있는 에셋이 Addressables에 등록되어 있지 않더라도, 해당 에셋은 자동으로 함께 Addressables 번들에 포함됩니다.**
  이 자동 포함 기능은 의도하지 않은 리소스 중복 문제를 유발할 수 있습니다.

* **서로 다른 번들의 프리팹이 동일한 비등록 에셋을 참조하는 경우**,
  해당 에셋은 각 번들에 중복 포함되어 최종 빌드 크기가 증가할 수 있습니다.
  → **공통 리소스는 별도의 그룹으로 분리하거나 명시적으로 Addressables에 등록하여 관리해야 합니다.**

* **압축 방식에 따라 메모리 로딩 방식이 달라집니다.**

  * **LZMA**: 번들 내 에셋 하나라도 로드 시, 전체 번들이 메모리에 압축 해제된 상태로 올라옵니다. (큰 메모리 사용)
  * **LZ4 / Uncompressed**: 요청한 에셋만 메모리에 로드되며, 더 유연한 메모리 관리가 가능합니다.

---

요약하면, Addressables는 "등록"만으로는 원하는 동작을 보장하지 않으며, "참조 방식"과 "번들 구성", "압축 설정"에 따라 실제 메모리 및 빌드 결과가 달라집니다. 중복 포함 문제는 Addressables 사용 시 가장 흔한 실수이므로 주의가 필요합니다.

## 사용법
(1.19.19 버전 기준으로 작성)
1. Package Manager에서 Addressables를 추가한다.
2. Window > Assets Manager > Addressables > Groups
3. Create Addressables Settings 클릭
4. Addressables로 관리할 에셋을 드래그로 그룹에 추가 (혹은 에셋 선택 후 Inspector에서 Addressable 체크로 추가 가능)
5. 스크립트로 해당 에셋 로드

추가된 에셋은 해당 에셋의 주소를 이용해 로드할 수 있다. (기본 값은 에셋 경로)

만약 프리팹을 로드하면 연결된 모든 에셋을 같이 로드한다. (프리팹 속 스크립트 **성능 저하**: Addressables는 원격 서버에서 에셋을 다운로드 받아야 하는 경우가 많으므로 네트워크 상태에 따라 성능 저하가 발생할 수 있습니다.  
또한 에셋 압축과 해제 과정에서 CPU와 GPU에 부담이 가중될 수 있습니다.

아래는 사용자가 작성한 Addressables의 "특징" 부분을 더 명확하고 실용적으로 다듬은 내용입니다:

## 특징 (정리 및 주의사항)

* **씬에서 직접 참조한 에셋은 Addressables 등록 여부와 관계없이 빌드에 포함됩니다.**
  이 경우, 동일한 에셋이 두 번 저장될 수 있습니다.
  (예: 씬 직접 참조용 / Addressables 로드용 → 중복 저장)

* 따라서 **Addressables에 등록한 에셋은 반드시 `Addressables.LoadAssetAsync<T>()` 등의 API로만 로드해야 중복 포함을 방지할 수 있습니다.**
  에디터나 코드에서 직접 참조할 경우, 해당 에셋은 Addressables 외에도 기본 빌드에도 포함됩니다.

* **Addressables 프리팹이 참조하고 있는 에셋이 Addressables에 등록되어 있지 않더라도, 해당 에셋은 자동으로 함께 Addressables 번들에 포함됩니다.**
  이 자동 포함 기능은 의도하지 않은 리소스 중복 문제를 유발할 수 있습니다.

* **서로 다른 번들의 프리팹이 동일한 비등록 에셋을 참조하는 경우**,
  해당 에셋은 각 번들에 중복 포함되어 최종 빌드 크기가 증가할 수 있습니다.
  → **공통 리소스는 별도의 그룹으로 분리하거나 명시적으로 Addressables에 등록하여 관리해야 합니다.**

* **압축 방식에 따라 메모리 로딩 방식이 달라집니다.**

  * **LZMA**: 번들 내 에셋 하나라도 로드 시, 전체 번들이 메모리에 압축 해제된 상태로 올라옵니다. (큰 메모리 사용)
  * **LZ4 / Uncompressed**: 요청한 에셋만 메모리에 로드되며, 더 유연한 메모리 관리가 가능합니다.

---

요약하면, Addressables는 "등록"만으로는 원하는 동작을 보장하지 않으며, "참조 방식"과 "번들 구성", "압축 설정"에 따라 실제 메모리 및 빌드 결과가 달라집니다. 중복 포함 문제는 Addressables 사용 시 가장 흔한 실수이므로 주의가 필요합니다.

## 사용법
(1.19.19 버전 기준으로 작성)
1. Package Manager에서 Addressables를 추가한다.
2. Window > Assets Manager > Addressables > Groups
3. Create Addressables Settings 클릭
4. Addressables로 관리할 에셋을 드래그로 그룹에 추가 (혹은 에셋 선택 후 Inspector에서 Addressable 체크로 추가 가능)
5. 스크립트로 해당 에셋 로드

추가된 에셋은 해당 에셋의 주소를 이용해 로드할 수 있다. (기본 값은 에셋 경로)

만약 프리팹을 로드하면 프리팹에서 사용하는 모든 에셋을 같이 로드한다. (프리팹 속 스크립트에 연결된 다른 에셋들도 재귀적으로 모두)
``` c#
Addressables.LoadAssetAsync<Texture2D>("Assets/Texture/test.png").Completed += (handle) =>
{
    _rawImage.texture = handle.Result;
};
```

혹은 AssetReference변수를 Inspector에서 할당해서 불러올 수도 있다.
``` c#
[SerializeField]
private AssetReference _assetReference;

[SerializeField]
private RawImage _rawImage;

private AsyncOperationHandle _textureHandle;
    
private void Start()
{
    Addressables.LoadAssetAsync<Texture2D>(_assetReference).Completed += (handle) =>
    {
        _textureHandle = handle;
        _rawImage.texture = handle.Result;
    };
}
```

메모리 해제는 Release 메서드로 진행하면 된다.

프리팹 해제 시 로드 시 같이 로드했던 연결된 모든 에셋을 같이 해제한다. (만약 다른 곳에서 사용 중이라면 당연히 해제 안됨)

만약 에셋을 오브젝트에서 사용 중이라면 해당 오브젝트를 파괴 후 해제해야 한다.
``` c#
Addressables.Release(_textureHandle);   // 핸들을 맴버 변수로 가지고 있어야 한다.
```

## 원격 에셋 업로드, 다운
서버에서 에셋을 로드하기 위해서는 다음과 같은 설정이 필요하다.
1. Addressables Profiles 창에서 Remote 주소를 Custom 설정으로 변경 후 LoadPath를 `서버주소/[BuildTarget]` 로 변경한다.
2. AddressableAssetSettings 에서 Build Remote Catalog 체크 후 Builds & Load Paths를 Remote로 설정한다.
3. 서버에 업로드할 Group 선택 후 Build & Load Paths를 Remote로 설정한다.
4. Addressables Groups 창에서 Build > New Build > Default Build Script 메뉴로 빌드한다.
5. 프로젝트 경로/ServerData 폴더 속에 있는 폴더를 서버에 업로드 한다.
6. 에디터에서 테스트 하기 위해서 Play Mode Script를 Use Existing Build로 변경한다.

이후 로드를 실행하면 자동으로 서버에서 해당 에셋을 다운로드 받아 캐싱 후 로드한다.

로드하지 않고 다운만 받고 싶다면 다음과 같은 코드를 사용할 수 있다.
``` c#
// 에셋의 다운로드 용량을 비동기적으로 확인합니다.
Addressables.GetDownloadSizeAsync(_assetReference).Completed += (handle) =>
{
    if (0 < handle.Result)
    {
        Debug.Log($"Download size: {handle.Result / 1048576f} MB");
    }
    else
    {
        Debug.Log("이미 다운 완료");
    }
};

// 에셋 다운로드 및 캐싱
Addressables.DownloadDependenciesAsync(_assetReference).Completed += (handle) =>
{
    PrintLog("다운 완료: " + handle.Status.ToString());
};

// 다운로드 받은 캐시 삭제
Addressables.ClearDependencyCacheAsync(_assetReference);
```
에셋 하나가 아니라 Label 단위로 다운 받기 위해서는 GetDownloadSizeAsync(string label명) 을 넣어주면 된다.

이미 다운 받을 에셋은 다운받지 않고 변경된 에셋이 있으면 새로 다운 받는다. (번들 단위)

다운로드 받은 에셋은 임시 폴더에 저장되기 때문에 영구적인 저장이 아니다.  
물론 앱 데이터 삭제를 하지 않는 이상 삭제될 일을 거의 없다.  

그래서 변경된 에셋을 계속 다운 받으면 기존 에셋이 삭제되지 않고 지속적으로 누적될 수 있다.