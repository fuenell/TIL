# Addressables
Addressables은 에셋을 그룹 단위로 묶어서 관리해주는 라이브러리다.  
이를 활용해 메모리 관리와 빌드 이후 에셋 교체가 가능하다.

## 장점

- **메모리 관리**: Addressables를 사용하면 에셋을 필요할 때만 로드하고 해제할 수 있으므로 메모리 부담을 줄일 수 있습니다.  
또한 에셋을 그룹으로 묶어서 관리할 수 있어서 의존성 문제를 해결할 수 있습니다.

- **콘텐츠 업데이트**: Addressables를 사용하면 에셋을 원격 서버에 저장하고 빌드 이후에도 교체할 수 있으므로 콘텐츠 업데이트가 용이합니다.  
또한 에셋의 주소를 통해 로드하기 때문에 코드 수정 없이 에셋을 변경할 수 있습니다.

- **플랫폼 호환성**: Addressables는 다양한 플랫폼에서 동작하며, 플랫폼별로 최적화된 에셋을 제공할 수 있습니다. 예를 들어,  
모바일에서는 저해상도의 텍스처를 사용하고 PC에서는 고해상도의 텍스처를 사용하는 등의 설정이 가능합니다.

## 단점

- **학습**: Addressables는 AssetBundles와 비슷하지만 다른 개념과 용어들이 많으므로 새롭게 학습해야 하는 부분이 있습니다.  
예를 들어, 그룹 스키마, 프로파일 변수, 카탈로그 등의 개념을 이해해야 합니다.

- **디버깅 어려움**: Addressables는 비동기 로딩 방식을 사용하므로 디버깅이 어려울 수 있습니다.  
예외 처리나 오류 메시지가 명확하지 않거나, 로드 시간이 길거나, 메모리 누수가 발생하는 등의 문제가 발생할 수 있습니다.

- **성능 저하**: Addressables는 원격 서버에서 에셋을 다운로드 받아야 하는 경우가 많으므로 네트워크 상태에 따라 성능 저하가 발생할 수 있습니다.  
또한 에셋 압축과 해제 과정에서 CPU와 GPU에 부담이 가중될 수 있습니다.

## 사용법
(1.19.19 버전 기준으로 작성)
1. Package Manager에서 Addressables를 추가한다.
2. Window > Assets Manager > Addressables > Groups
3. Create Addressables Settings 클릭
4. Addressables로 관리할 에셋을 드래그로 그룹에 추가 (혹은 에셋 선택 후 Inspector에서 Addressable 체크로 추가 가능)
5. 스크립트로 해당 에셋 로드

추가된 에셋은 해당 에셋의 주소를 이용해 로드할 수 있다. (기본 값은 에셋 경로)
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

다운로드 받은 에셋은 캐시 폴더에 저장되기 때문에 영구적인 저장이 아니다.  
만약 영구적으로 저장하기 위해서는 따로 서버에서 에셋을 다운로드 후 저장하는 코드를 작성해야할 것 같다.
