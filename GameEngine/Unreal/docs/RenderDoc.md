# 언리얼 렌더독 연결
언리얼에서 그래픽스 디버깅용 프로그램 렌더독을 연결하는 방법 (UE5 기준)

0. 렌더독 설치
알아서 설치

1. 플러그인 추가
Edit > Plugins > Render Doc 검색 후 추가 및 재실행
<img width="911" height="463" alt="Image" src="https://github.com/user-attachments/assets/8ebd4349-059f-44e1-81e9-c701d7542fea" />

2. 프로젝트 설정에서 경로 및 자동 연결 설정
- Auto attach on startup 옵션 체크
- RenderDoc executable path 에 렌더독 프로그램 폴더 경로 설정 (qrenderdoc.exe 가 있는 폴더)
<img width="872" height="557" alt="Image" src="https://github.com/user-attachments/assets/70e47295-213a-4caf-8c07-192c9f2f43ae" />

3. 이후 재시작 후 정상적으로 연결되면 뷰포트 상단에 렌더독 캡처 버튼이 생긴다
(버튼이 보이지 않으면 OutputLog에서 연결 실패 사유 확인)
<img width="1086" height="459" alt="Image" src="https://github.com/user-attachments/assets/8691dfcd-0430-48c5-9c66-c197c8565a93" />

4. 이제 캡처하고 싶은 장면에서 해당 버튼을 누르면 자동으로 렌더독이 켜지면서
(렌더독을 미리 켜둘 필요 없음)

### 필터
1. Event Browser에서 수많은 요청들이 있는데 Fiter에서 `draw`를 검색하면 그나마 보기 편함

### PS 디버깅
만약 PS를 HLSL로 디버깅하고 싶으면 아래 절차를 모두 수행하면 된다
(그냥 디버깅하면 어셈블리로 나옴)

1. 프로젝트를 끄고 `.uproject` 파일 바로가기를 생성한다
2. 우클릭 > 속성 > 대상 끝에 `-d3ddebug -r.Shaders.Symbols=1 -r.Shaders.Optimize=0` 를 추가한다

예시 `C:\Users\Demo.uproject -d3ddebug -r.Shaders.Symbols=1 -r.Shaders.Optimize=0`

3. 설정한 바로가기로 프로젝트를 실행한다 (셰이더 컴파일 오래 걸림)

4. 언리얼 콘솔창에 `r.Shaders.Symbols` 명령을 입력해서 `1` 로 잘 설정되었는지 확인한다.
```
Cmd: r.Shaders.Symbols
HISTORY
Constructor: 0
SystemSettingsIni: 1
r.Shaders.Symbols = "1"      LastSetBy: SystemSettingsIni
```

5. 렌더독에서 셰이더 경로를 지정해준다
렌더독 > Tools > Settings > Core > Shader debug search paths > `프로젝트경로\Saved\ShaderSymbols` 추가

6. 이제 Event Browser 에서 원하는 드로우콜을 찾아서 Texture View에서 우클릭으로 선택 후 픽셀 디버깅 하면됨