# 예약된 폴더 이름 [#Manual](https://docs.unity3d.com/6000.0/Documentation/Manual/SpecialFolders.html)
Unity는 프로젝트 내에서 폴더의 이름에 따라 특정한 기능을 수행하도록 예약된 폴더 이름을 사용합니다. 아래는 주요 예약 폴더와 그 목적에 대한 설명입니다.

|폴더명|설명|
|-|-|
|Editor|에디터 전용 스크립트를 보관하는 폴더입니다. 에디터 스크립트는 런타임 빌드에 포함되지 않으며, Unity 에디터 환경에서만 동작합니다.|
|Resources|런타임 시 스크립트를 통해 동적으로 에셋을 로드할 수 있도록 에셋을 보관하는 폴더입니다. Resources.Load 함수를 사용하여 접근합니다.|
|Plugins|네이티브 플러그인 파일(.dll, .so 등)을 보관하는 폴더입니다. 플랫폼별 플러그인 로드에 사용되며 스크립트 컴파일 순서에도 영향을 줍니다.|
|StreamingAssets|빌드 파일에 원본 포맷 그대로 포함되어야 하는 에셋을 저장하는 폴더입니다. 동영상, JSON 파일 등의 로우 데이터를 저장할 때 유용합니다.|

그외 `Gizmos`, `Editor Default Resources`, `Standard Assets` 등이 있습니다.
