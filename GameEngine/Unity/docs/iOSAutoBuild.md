# iOS 빌드 자동화
iOS 빌드 및 배포를 자동화 하는 과정 정리 (GitLab Runner 기준)


## 1. 준비물 (이게 없으면 진행이 불가)
- xcode-select(Xcode CLI)가 설치된 Mac PC + (fastlane도 설치)
- 결제된 Apple 개발자 계정
- Apple Store Connect에 번들 및 앱 정보 등록


## 2. 유니티 -> Xcdoe 빌드 자동화
Xcode로 빌드는 Mac이 아니더라도 가능하기 때문에 [unityci/editor](https://hub.docker.com/r/unityci/editor/tags) 이미지로 ios 빌드를 진행한다.
`unityci/editor:${UNITY_VERSION}-ios-${IMAGE_VERSION}`

``` yml
ios-build:
  image: unityci/editor:2022.3.18f1-ios-3.1.0   # 해당 도커 이미지로 러너 실행
  cache:    # 빌드 속도 최적화를 위해 Library 폴더 캐싱
    key: "iosbuildTest-2022.3.18f1-ios"
    paths:
      - Library/
  script:   # 빌드 진행
    - chmod +x ./ci/git-activate.sh && ./ci/git-activate.sh # 라이선스 다운
    - chmod +x ./ci/build.sh && ./ci/build.sh   # 빌드 진행
    - cp -r ios/fastlane Builds     # 추후 설명
  artifacts:    # xcode 빌드 파일 업로드
    paths:
      - Builds
    when: on_success
    expire_in: 30 days
```



## 3. App Store Connect API 발급 [#링크](https://appstoreconnect.apple.com/access/integrations/api)
만약 조직에 포함되거나 직접적으로 ID와 비밀번호를 사용해서 로그인하기 원치 않는다면 api 팀 키, 개별 키를 발급 받아서 로그인 대신 해당 키를 사용할 수 있다. 키를 발급 받아 아래와 같은 양식으로 생성해서 프로젝트 경로에 두면 사용할 수 있다. (하지만 시크릿으로 선언해두고 동적으로 생성하는 편이 정석인 것 같다)
```json
{
    "key_id": " 키 id",
    "issuer_id": "이슈어 id",
    "key": "-----BEGIN PRIVATE KEY-----\n키 내용\n-----END PRIVATE KEY-----",
    "duration": 1200,
    "in_house": false
}
```



## 4. pod 초기화
만약 프로젝트의 종속성이 필요하면 Mac에서 `Cocoapod`을 설치하고 `pod install`을 xcode로 빌드된 프로젝트 경로에서 실행한다. 그러면 Unity-iPhone.xcworkspace 파일이 생성된다. 이후 배포는 workspace 파일로 진행해야한다.



## 5. fastfile 초기화
Mac에 xcode로 빌드된 프로젝트를 가져와서 해당 경로에서 터미널을 연 후 `fastlnae init`으로 fastlane을 초기화한다. 아니면 그냥 아래 내용으로 `./fastlane/` 경로에 파일을 생성해두면 된다.

[Fastfile]
```
default_platform(:ios)

platform :ios do
  desc "Push a new beta build to TestFlight"

  lane :beta do 

    sync_code_signing(type:"appstore", readonly: is_ci)   # match 저장소에서 인증서를 읽기 전용으로 가져온다

    # increment_build_number  # 빌드 번호 자동 증가 (필요 없을 시 제외)

    # 앱 빌드
    build_app(
      build_path: "./output",   # 기본 경로는 권한 문제가 발생할 수도 있어서 지정
      workspace: "Unity-iPhone.xcworkspace",    # 종속성 필요 시 workspace로 빌드 진행
      scheme: "Unity-iPhone"
    )

    # 앱 배포
    upload_to_testflight(
      api_key_path: "./fastlane/key.json"
      # , groups: ["youcanstar"]    # 배포 타겟 그룹 (내부 테스트는 필요 없다)
    )
  end
end
```

[Appfile]
```
app_identifier "com.앱 번들" # 앱의 번들 ID
team_id "팀 id"              # 팀 ID (Apple Developer Account의 팀 ID)
```



## 6. fastlane match 설정
apple 앱을 빌드 하려면 인증서가 필요한데 CLI 환경에서는 자동인증을 할 수 없기 때문에 fastlane에서 인증서를 git에 올려두어 빌드 시 해당 git을 clone 받아서 인증을 진행해주는 기능이 있는데 이게 match이다. git 저장소에 리포지토리를 미리 만들어두어야 한다.

`fastlane match init`으로 Matchfile을 생성하거나 아래 내용으로 바로 Matchfile을 만들어두어도 된다.
```
git_url("http://oauth2:[인증서]@git.ycs.com/mobilegames/ios-fastlane-match")

storage_mode("git")

type("appstore")	# 인증서 타입 testflight 및 appstore 배포 용

api_key_path("./fastlane/key.json")		# 미리 만들어둔 App Store Connect API 키
```

이후 `fastlane match`를 실행하면 인증서 생성과 git에 인증서 업로드가 자동으로 진행된다.
중간에 요구하는 인증서용 암호는 하나로 통일하면 좋다.

만약 잘 생성되지 않으면 `fastlane match nuke distribute` 으로 distribute 에 해당하는 인증서를 모두 삭제 후 다시 match를 진행한다.



## 7. 인증서 Unity에 등록
애플 개발자 인증서에 들어가보면 match를 통해 생선된 인증서와 프로필이 있다.
그 중 프로필을 다운 받아 유니티에서 `Project Settings > Player > iOS > Automatically Sign 해제`, 이후 `iOS Provisioning Profile - Browse` 로 다운 받은 프로필 선택해서 프로필 ID를 등록한다. 추후 자동 빌드 시 어떤 프로필과 인증서를 사용한 건지 지정한 작업이다. (실제 프로필이 유니티에 등록된 것이 아니고 ID만 등록)

그리고 Bundle Identifier과 Signing Team ID 도 같이 등록한다. (애플 개발자 프로필 편집 메뉴에서 team id 확인 가능)



## 8. 빌드 진행 deploy yml
그렇게 만들어둔 Fastfile, Appfile, MatchFile, ApiKey를 프로젝트 경로 `ios/fastlane`에 넣어두고 빌드 시 `- cp -r ios/fastlane Builds` 를 통해서 미리 fastlane 파일들을 빌드 파일에 포함시킨다. 이후에는 아래 내용을 mac에서 실행하게하면 된다.
이 작업은 도커에서 진행할 수 없다. (도커에서 mac 환경을 실행할 수 없기 때문에) 때문에 shell 환경 mac runner를 생성해두고 진행해야한다.

시크릿에 `MATCH_PASSWORD` 를 미리 등록해두어야 한다. match 실행 시 설정했던 암호를 등록한다.

```yml
ios-deploy:
  stage: deploy
  dependencies: 
    - ios-build   # artifacts 의존 설정
  variables:
    GIT_STRATEGY: none
  script:
    - cd Builds
    - pod install
    - fastlane ios beta
```

진행 도중 mac에서 mac 기기 암호를 요구할 시 항상 허용으로 체크해주어야 한다.

missing description 에러 발생 시
웹사이트에서 앱 설명을 추가해야 됨

missing requied data 에러 발생 시
웹사이트에서 심사 정보 연락처와 빈칸 모두 채우기