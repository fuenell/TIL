# GitHub Action [#GameCI](https://game.ci/docs/github/getting-started)
GitHub Action을 이용해 Unity 프로젝트를 자동으로 빌드하는 기능 추가 (GameCI에서 많은 부분 참고)

## 1. Unity License 활성화
외부 컴퓨터로 Unity 프로젝트를 빌드하기 위해서는 아래 작업을 통해 해당 컴퓨터에 Unity License를 활성해주어야 한다.

### 1-1. License 활성화 yml 파일 추가
Unity 프로젝트가 있는 GitHub에 /.github/workflows/activation.yml 폴더 및 파일을 생성하고 다음 내용을 작성한다.
``` yml
name: Acquire activation file
on:
  workflow_dispatch: {}
jobs:
  activation:
    name: Request manual activation file 🔑
    runs-on: ubuntu-latest
    steps:
      - name: Request manual activation file
        id: getManualLicenseFile
        uses: game-ci/unity-request-activation-file@v2

      - name: Expose as artifact
        uses: actions/upload-artifact@v2
        with:
          name: ${{ steps.getManualLicenseFile.outputs.filePath }}
          path: ${{ steps.getManualLicenseFile.outputs.filePath }}
          
      - name: Read Me
        run: |
          echo '1. Sunmmary로 이동 후 Artifacts에 업로드된 .alf 파일 다운'
          echo '2. 다음 링크 접속 -> https://license.unity3d.com/manual'
          echo '3. 위 사이트에 다운받은 alf 등록 후 ulf 파일 다운'
          echo '4. Settings > Actions > Secrets > New repository secret (아래 3개 변수 추가)'
          echo 'UNITY_LICENSE - (ulf 파일의 내용 복사)'
          echo 'UNITY_EMAIL - (Unity 이메일)'
          echo 'UNITY_PASSWORD - (Unity 비밀번호)'
```
### 1-2. alf 파일 생성 작업 실행
![image](https://user-images.githubusercontent.com/37904040/187933147-0f2b1fc9-ead1-4ada-ba57-ac79c17b7225.png)

작업이 완료되면 해당 작업을 클릭해 Artifacts에 업로드된 .alf 파일을 다운한다.

![image](https://user-images.githubusercontent.com/37904040/187934153-27ac23cb-8745-4912-8340-eff82fb76984.png)

### 1-3. ulf 파일 생성
https://license.unity3d.com/manual

위 사이트에 접속해 1-2에서 다운 받은 .alf파일 등록 및 License 선택 후 .ulf 파일을 다운한다.

### 1-4. Secret 등록
Settings > Actions > Secrets > New repository secret 메뉴로 다음 변수 3개 입력한다.
- UNITY_LICENSE - (ulf 파일의 내용 복사)
- UNITY_EMAIL - (Unity 이메일)
- UNITY_PASSWORD - (Unity 비밀번호)

## 2. 빌드

### 2-1. 빌드 yml 파일 추가
Unity 프로젝트가 있는 GitHub에 /.github/workflows/main.yml 폴더 및 파일을 생성하고 다음 내용을 작성한다.
``` yml
name: Build project

on:
  push:     # 빌드 작업 실행조건은 변경 가능

jobs:
  build:
    name: Build for ${{ matrix.targetPlatform }}
    runs-on: ubuntu-latest
    strategy:
      fail-fast: false
      matrix:
        targetPlatform:
          - StandaloneWindows       # Target Platform
    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0
          lfs: true

      - uses: actions/cache@v2
        with:
          path: Library
          key: Library-${{ matrix.targetPlatform }}
          restore-keys: Library-

      - uses: game-ci/unity-builder@v2
        env:
          UNITY_LICENSE: ${{ secrets.UNITY_LICENSE }}
        with:
          targetPlatform: ${{ matrix.targetPlatform }}
          customParameters: -BuildOptions CompressWithLz4
          # projectPath: ProjectPath/       # GitHub에 Push된 Unity 프로젝트의 경로가 최상위가 아니리면 설정 필요 

      - uses: actions/upload-artifact@v2
        with:
          name: build-${{ matrix.targetPlatform }}
          path: build/${{ matrix.targetPlatform }}
```

### 2-2. 프로젝트 빌드
해당 yml 파일이 등록된 후 Push가 발생할 때 마다 자동으로 프로젝트가 빌드되어 Artifacts에 업로드된다.