# ironSource LevelPlay
ironSource의 LevelPlay는 Unity에서 지원해주는 광고 미디에이션 SDK 이다.

광고 미디에이션이란 여러 광고 플랫폼을 통합 및 관리해주는 것이다.  
예를 들어 Unity Ads와 Google AdMob을 동시에 사용하는 경우, 각 광고 플랫폼을 관리하는 것이 아니라 미디에이션을 통해 다양한 광고 네트워크를 하나의 인터페이스로 통합하여 관리할 수 있다.

## 세팅법 [#Docs](https://developers.is.com/ironsource-mobile/unity/levelplay-starter-kit/#step-1) / [#가이드 영상](https://youtu.be/sU5njx1jn8w?si=Nu3tSzrrOcsS8aOY) / [#추가 가이드 영상](https://youtu.be/povu-YG6bX0?si=FgyvssEithyVyzMz)

`*영상 시청 추천`

#### 1. 아어언 소스 사이트 설정
1. 아이언 소스 계정 생성
2. 앱 생성 (광고 종류 활성화)
3. 광고 network 추가 (Unity Ads, Google AdMob)

#### 2. Unity Ads 연결
1. 프로젝트에서 Project Settings > Services > 프로젝트 연결
2. 유니티 클라우드에서 프로젝트 선택 후 Unity Ads Monetization으로 이동
3. Get started(Launch) 클릭
4. Mediation에서 Unity LevelPlay선택 후, Binding 옵션 선택해 완료
5. Placements에서 배너, 전면, 리워드 옵션 추가
6. 아이언소스에서 SDK networks 에서 Unity Ads의 Placement ID 입력
7. 재무 정보를 추가해 Unity Ads 계정 인증

#### 3. Google AdMob 연결
1. Google AdMob 사이트에서 계정 생성
2. 앱 등록
3. Ad unit 추가
4. id 복사 후 아이이언 소스에서 Placement ID 입력
5. 결제방법을 등록해 계정 인증

#### 4. 유니티 설정
1. IL2CPP 옵션 설정
2. 타겟 아키텍쳐 모두 체크
3. .NET 4.x 으로 설정
4. 커스텀 minifest, gradleTemplate 설정

#### 5. 아이언 소스 SDK 추가
1. SDK 패키지 추가
2. Ads Mediation > Intergration Manager > [Unity Ads, Google AdMob] 설치
3. Assets > Mobile Dependency Resolver > Android Resolver > Resolve 실행

#### 6. 광고 호출 및 테스트
1. 테스트 기기에 자신의 기기 등록 (아니면 광고 정지 당할 수 있음)
2. 데모씬 빌드해서 광고가 정상적으로 표시되는지 확인 (에디터에서 확인이 안됨)



## 에러 / 버그 모음
### 중요
```
모든 에러 발생 시 
Publishing Setting에서 Custom Keystore 사용 중이라면 Password 확인
```

### platform이 달라서 Resolve 실패
build platform을 android로 전환


### Resolve 실패1 Win32Exception 에러
@@@@ greadlew.bat 파일 첨부 @@@@
1. `프로젝트\Temp\PlayServicesResolverGradle\` 경로에 `greadlew.bat` 파일을 넣습니다.
2. Unity에서 Project창에 있는 Assets폴더 우클릭 후 Reimport 실행
3. `Assets > Mobile Dependency Resolver > Android Resolver > Resolve` 실행


### greadlew.bat 으로 Resolve 이후 빌드 시,  실패 에러
1. `greadlew.bat` 파일을 삭제 후, Assets폴더 Reimport 실행


### Resolve 실패2 100%에서 fail
아이언 소스 SDK와 유니티 버전을 바꿔서 다시 시도  
[#아이언소스 Unity SDK 아카이브](https://github.com/ironsource-mobile/Unity-sdk/tags)  


### 호환 문제로 빌드 실패 (A failure occurred while executing)
```
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
Note: Some input files use or override a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
Note: Some input files use unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
```
타겟 SDK level과 gradle 버전이 호환되지 않는듯한 느낌
gradle 버전을 올려서 다시 시도

### 마지막으로 확인한 호환되는 버전
|Unity|ironSource|UnityAds|Google AdMob|비고|
|-|-|-|-|-|
|2022.2.21|7.6.1|4.3.38.0|4.3.54.0||
|2022.3.16|7.6.1|4.3.38.0|4.3.54.0||
|2022.3.16|7.6.1|4.3.38.0|**4.3.57.0**|AdMob Resolve 후 1.7.1 삭제|


### 화면을 껐다 켜면 배너 광고가 사라지는 현상 [#Unity 이슈 트래커](https://issuetracker.unity3d.com/issues/android-admobsdk-banner-ad-disappears-when-the-device-goes-to-the-home-screen-and-returns-to-the-application)
2022.3 버전 사용 시 2022.3.16f1 까지 업그레이드  
2022.2.21 버전에서는 버그가 해당 버그가 발생하지 않음


### 구글 플레이 콘솔 AD_ID 관련 문제
AndroidManifest.xml 에 `<uses-permission android:name="com.google.android.gms.permission.AD_ID" />` 가 선언되어 있어야됨
Unity Ads 가 설치되어 있으면 충돌 발생할 수 있음


### 구글 플레이 콘솔 앱 업데이트 거부
```
Google Play 사용자 데이터 정책의 데이터 보안 섹션: 데이터 보안 양식 잘못됨

com.github.adxcorp.ADXLibrary_Android:adx-library-ironSource
com.github.adxcorp.ADXLibrary_Android:adx-library-ironSource: SDK를 삭제하거나,
SDK 제공업체에서 제공하는 경우 정책을 준수하는 버전의 SDK로 업그레이드하는 것이 좋습니다.
com.ironsource.sdk:mediationsdk com.ironsource.sdk:mediationsdk: SDK를 삭제하거나,
SDK 제공업체에서 제공하는 경우 정책을 준수하는 버전의 SDK로 업그레이드하는 것이 좋습니다.
```
아이언 소스 업데이트? 유니티 Ads 삭제?