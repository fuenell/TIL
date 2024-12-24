# iOS 빌드 자동화
iOS 빌드 및 배포를 자동화 하는 과정 정리 (GitLab Runner 기준)


## 1. 준비물 (이게 없으면 진행이 불가)
- xcode-select(Xcode CLI)가 설치된 Mac PC
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
만약 조직에 포함되거나 직접적으로 ID와 비밀번호를 사용해서 로그인하기 원치 않는다면 api 팀 키, 개별 키를 발급 받아서 로그인 대신 해당 키를 사용할 수 있다.


## 4. fastlane match 설정
apple 앱을 빌드 하려면 인증서가 필요한데 cli 환경에서는 자동인증을 할 수 없기 때문에 fastlane에서 인증서를 git에 올려두어 빌드 시 해당 git을 clone 받아서 인증을 진행해주는 기능이 있는데 이게 match이다.
```
git_url("http://oauth2:[인증서]@git.ycs.com/mobilegames/ios-fastlane-match")

storage_mode("git")

type("appstore")	# 인증서 타입 appstore은 testflight 및 appstore 배포 시 사용

api_key_path("./fastlane/key.json")	# 프리모엠 애플 팀 키
```