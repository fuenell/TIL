# GooglePlayGames Unity 연동 [#Guide](https://developer.android.com/games/pgs/unity/unity-start?hl=ko)
가이드를 기반으로 필요한 내용을 추가했다.

## 1. 구글 계발자 계정 생성
https://play.google.com/console 에서 계발자 계정 생성 및 등록비 결제(최초 1회)

## 2. 구글 콘솔에서 앱 만들기
앱 또는 게임 선택지에서 게임 선택

## 3. Google Cloud 콘솔에서 프로젝트 생성
https://console.cloud.google.com/projectcreate
Google API를 사용하기 위해서 구글 클라우드에서 프로젝트 생성

## 4. Play 게임즈 서비스 활성화
구글 플레이 콘솔 - 앱 선택 - 성장 - Play 게임즈 서비스 - 설정 및 관리 - 설정
방금 생성한 프로젝트 선택 및 저장

## 5. OAuth 동의 화면 구성
따라서 쭉 하면됨

## 6. 유니티에서 keystore 생성
Project Settings - Publishing Settings - Costom Keystore - 키 생성
키 생성 후 안드로이드 스튜디오에 포함된 keytool 로 SHA1 지문 확인 및 복사
```
"안드로이드 스튜디오 경로\jre\bin\keytool" -list -v -keystore "키 경로\user.keystore"
```

## 7. OAuth 클라이언트 ID 만들기
https://console.cloud.google.com/apis/credentials/oauthclient
에서 OAuth 클라이언트 ID 만들기
Android / 프로젝트명 / 패키지명(유니티에서 적용한 Package Name) / 6단계에서 복사한 SHA1 지문

## 8. 사용자 인증 정보 추가
7단계에서 만든 OAuth 클라이언트를 선택해서 저장

## 9. 유니티에서 Android setUp 실행
Window - Google Play Games - Setup - Android setup
Resources Definition 은 Play 게임즈 서비스 설정 페이지의 사용자 인증 정보에서 리소스 보기 클릭 후 Android(XML) 그대로 복사해서 넣음
Client ID 는 사용자 인증 정보의 ID를 클릭해서 나오는 수정 페이지에서 확인 가능




