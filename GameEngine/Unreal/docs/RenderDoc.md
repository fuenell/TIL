# UE5에서 RenderDoc 연결하기 (그래픽 디버깅)

## 0) RenderDoc 설치

* 설치는 생략합니다. (RenderDoc만 정상 설치되어 있으면 됩니다)

---

## 1) 플러그인 추가

1. **Edit > Plugins**
2. 검색창에 **RenderDoc** 검색
3. 플러그인 **Enable(활성화)** 후 **에디터 재실행**

![플러그인 추가](https://github.com/user-attachments/assets/8ebd4349-059f-44e1-81e9-c701d7542fea)

---

## 2) 프로젝트 설정에서 RenderDoc 경로 + 자동 연결 설정

1. **Project Settings**에서 RenderDoc 관련 섹션으로 이동
2. 아래 2개 설정을 해줍니다.

* **Auto attach on startup** ✅ 체크
* **RenderDoc executable path**: `qrenderdoc.exe`가 있는 폴더 경로 지정

![프로젝트 설정](https://github.com/user-attachments/assets/70e47295-213a-4caf-8c07-192c9f2f43ae)

---

## 3) 재시작 후 연결 확인

* 에디터 재시작 후 정상적으로 연결되면 **뷰포트 상단에 RenderDoc 캡처 버튼**이 생깁니다.

![캡처 버튼](https://github.com/user-attachments/assets/8691dfcd-0430-48c5-9c66-c197c8565a93)

* 버튼이 안 보이면:

  * **Output Log**에서 RenderDoc 연결 실패 사유를 확인하세요.

---

## 4) 프레임 캡처

* 캡처하고 싶은 장면에서 **캡처 버튼 클릭**
* 자동으로 **RenderDoc이 실행되면서 프레임 캡처가 완료**됩니다.
* RenderDoc을 **미리 켜둘 필요는 없습니다.**

---

# 추가 팁

## 필터

* **Event Browser**에 요청이 너무 많습니다.
* **Filter**에 `draw`를 입력하면 그나마 보기 편합니다.

---

# PS(HLSL) 디버깅 설정 (어셈블리 말고 HLSL로 보기)

그냥 디버깅하면 어셈블리로 나올 수 있습니다. **아래 절차를 모두 적용**하면 HLSL로 디버깅할 수 있습니다.

## 1) 프로젝트 종료 후 `.uproject` 바로가기 생성

* 프로젝트(에디터)를 끈 상태에서 `.uproject` 파일 **바로가기**를 만듭니다.

## 2) 바로가기 실행 옵션 추가

1. 바로가기 우클릭 → **속성**
2. **대상(Target)** 맨 끝에 아래 옵션을 추가합니다.

추가할 옵션:

* `-d3ddebug -r.Shaders.Symbols=1 -r.Shaders.Optimize=0`

예시:

```txt
C:\Users\Demo.uproject -d3ddebug -r.Shaders.Symbols=1 -r.Shaders.Optimize=0
```

## 3) 해당 바로가기로 프로젝트 실행

* 이 설정으로 실행하면 **셰이더 컴파일이 오래 걸릴 수 있습니다.**

## 4) 콘솔에서 설정 적용 확인

* 언리얼 콘솔창에서 아래 명령으로 셰이더 심볼이 1로 설정되었는지 확인합니다.
`r.Shaders.Symbols`
```txt
Cmd: r.Shaders.Symbols
HISTORY
Constructor: 0
SystemSettingsIni: 1
r.Shaders.Symbols = "1"      LastSetBy: SystemSettingsIni
```

만약 0으로 뜬다면 프로젝트 캐시 파일 모두 삭제 후 다시 바로가기로 실행하세요.

## 5) RenderDoc에 Shader Symbols 경로 추가

1. RenderDoc 실행
2. **Tools > Settings > Core**
3. **Shader debug search paths**에 아래 경로 추가:

* `프로젝트경로\Saved\ShaderSymbols`

## 6) 픽셀 디버깅

1. **Event Browser**에서 원하는 드로우콜 찾기
2. **Texture View**에서 우클릭으로 대상 선택
3. **Pixel Debugging** 실행

