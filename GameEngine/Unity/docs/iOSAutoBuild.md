# iOS 빌드 자동화

GitLab Runner를 활용하여 iOS 빌드 및 배포를 자동화하는 과정을 단계별로 정리했습니다.

---

## 1. 준비물
자동화를 시작하기 전에 다음 항목을 준비해야 합니다:

- **Mac PC**: Xcode와 Xcode CLI(xcode-select)가 설치되어 있어야 합니다.
- **유료 Apple 개발자 계정**: 배포를 위해 필요합니다.
- **Apple Store Connect**: 앱 번들과 앱 정보를 등록해야 합니다.

---

## 2. Unity -> Xcode 빌드 자동화
Mac 환경이 아닌 곳에서도 Unity iOS 빌드를 실행할 수 있습니다. 이를 위해 [unityci/editor](https://hub.docker.com/r/unityci/editor/tags) Docker 이미지를 활용합니다.

### GitLab CI 설정
다음은 GitLab CI 파이프라인을 설정하는 예제입니다:

```yml
ios-build:
  image: unityci/editor:2022.3.18f1-ios-3.1.0  # Docker 이미지 지정
  cache:  # 빌드 속도 최적화를 위한 캐싱
    key: "iosbuildTest-2022.3.18f1-ios"
    paths:
      - Library/
  script:  # 스크립트를 사용하여 빌드 진행
    - chmod +x ./ci/git-activate.sh && ./ci/git-activate.sh  # Unity 라이선스 활성화
    - chmod +x ./ci/build.sh && ./ci/build.sh  # Unity 프로젝트 빌드
    - cp -r ios/fastlane Builds  # Fastlane 설정 복사
  artifacts:  # 빌드 결과물 업로드
    paths:
      - Builds
    when: on_success
    expire_in: 30 days
```

---

## 3. App Store Connect API 키 발급
App Store Connect API 키를 활용하면 계정 문제 없이 자동화를 더욱 효율적으로 구현할 수 있습니다.

### API 키 발급 절차
1. [App Store Connect API 페이지](https://appstoreconnect.apple.com/access/integrations/api)에 접속합니다.
2. API 키를 생성하고 다운로드한 후 아래 형식대로 JSON 파일을 생성합니다.

### JSON 파일 예제
```json
{
    "key_id": "키 ID",
    "issuer_id": "이슈어 ID",
    "key": "-----BEGIN PRIVATE KEY-----\n키 내용\n-----END PRIVATE KEY-----",
    "duration": 1200,
    "in_house": false
}
```
> **Tip:** 보안을 위해 JSON 파일을 환경 변수나 시크릿으로 관리하는 것이 좋습니다.

---

## 4. Pod 초기화
필요시 Cocoapods를 설정하여 프로젝트 종속성을 관리합니다.

1. Mac 터미널에서 `sudo gem install cocoapods` 명령어를 실행하여 Cocoapods를 설치합니다.
2. Xcode 프로젝트 디렉토리에서 `pod install`을 실행하여 `Unity-iPhone.xcworkspace` 파일을 생성합니다.
3. 이 workspace 파일을 사용해 빌드와 배포를 진행합니다.

---

## 5. Fastlane 초기화

Fastlane을 설정하여 빌드와 배포 작업을 자동화합니다.

### Fastfile 생성 및 작성
1. Xcode 프로젝트 디렉토리에서 터미널을 열고 `fastlane init`을 실행합니다.
2. 생성된 `Fastfile`을 아래와 같이 수정합니다. 처음부터 직접 파일을 생성해도 됩니다.

```ruby
default_platform(:ios)

platform :ios do
  desc "TestFlight로 새로운 베타 빌드 업로드"

  lane :beta do
    sync_code_signing(type: "appstore", readonly: is_ci)  # Match 인증서 동기화

    build_app(
      build_path: "./output",  # 권한 문제 방지를 위한 경로 설정
      workspace: "Unity-iPhone.xcworkspace",
      scheme: "Unity-iPhone"
    )

    upload_to_testflight(
      api_key_path: "./fastlane/key.json"
    )
  end
end
```

### Appfile 생성 및 작성
`Appfile`을 다음과 같이 작성합니다:

```ruby
app_identifier "com.앱번들"   # 앱 번들 ID
team_id "팀 ID"               # Apple 개발자 계정 팀 ID
```

---

## 6. Fastlane Match 설정

Fastlane의 Match 기능을 활용하여 인증서를 git 저장소에서 관리합니다.

### Matchfile 생성 및 작성
1. 인증서 저장용 git 저장소를 하나 생성합니다.
2. `fastlane match init` 명령어를 실행하거나, 아래와 같이 `Matchfile`을 직접 작성합니다:

```ruby
git_url("http://oauth2:[액세스 토큰]@[인증서 저장용 git 주소]")

storage_mode("git")

type("appstore")

api_key_path("./fastlane/key.json")
```

### 명령어 실행
1. `fastlane match` 명령어를 실행하면 인증서가 생성되고 Git 저장소에 업로드됩니다.
2. 인증서 암호는 통일하여 관리하면 편리합니다.

> **Tip:** 인증서가 잘 생성되지 않을 경우 `fastlane match nuke [type]` 명령어로 인증서를 삭제한 뒤 다시 시도하세요.

---

## 7. Unity 설정

1. Apple 개발자 페이지에서 생성된 인증서와 프로비저닝 프로필을 다운로드합니다.
2. Unity에서 다음 설정을 진행합니다:
   - `Project Settings > Player > iOS > Automatically Sign` 비활성화
   - `iOS Provisioning Profile - Browse`에서 다운로드한 프로필 선택
   - `Signing Team ID`와 `Bundle Identifier` 설정

---

## 8. 배포 자동화

GitLab CI를 이용해 Fastlane을 실행하여 iOS 빌드를 TestFlight에 배포합니다.

### GitLab CI 배포 단계 예제
```yml
ios-deploy:
  stage: deploy
  dependencies:  
    - ios-build  # 빌드 결과물 의존성 설정
  variables:
    GIT_STRATEGY: none
  script:
    - cd Builds
    - pod install
    - fastlane ios beta
```

> **Note:** Mac 환경에서 암호 입력을 요청받는 경우 "항상 허용"으로 설정해야 자동화가 원활히 진행됩니다.

---

## 자주 발생하는 오류 해결 방법

### Missing description 오류
App Store Connect 웹사이트에서 앱 설명을 추가하여 해결할 수 있습니다.

### Missing required data 오류
App Store Connect에서 심사 정보와 연락처 등의 필수 정보를 모두 입력하세요.

---