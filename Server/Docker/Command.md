# 도커 명령어 [#docs](https://docs.docker.com/engine/reference/commandline/cli/)

## 용어 정리
- 도커: 서버 환경을 분할? 및 관리하는 프로그램
- 이미지: 저장된 작업 환경
- 컨테이너: 이미지를 인스턴스화 한 것
- yml(Docker Compose): 도커 명령어 모음

이미지=클래스고 컨테이너=인스턴스 개념

### docker pull
이미지는 [Docker Hub](https://hub.docker.com/)에서 검색 후 다운된다.
```
# 이미지 다운
docker pull [이미지명]:[버전]
docker pull ubuntu:22.04
```

### docker image
```
# 이미지 목록 출력
docker image ls
```

### docker run
이미지를 컨테이너로 생성
```
# 이미지 목록 출력
docker run [옵션] [이미지:버전] [명령어]	// 해당 이미지가 없다면 다운 받고 컨테이너를 생성/실행 시킴
///
docker pw [-a]				// 컨테이너 출력 (-a는 종료된 컨터이너 포함)
docker exec [컨터이너명or컨테이너ID]		// 생성된 컨테이너 실행
docker build -t [이미지명] .			// 해당 위치에 도커 파일을 읽어 이미지를 생성한다 (도커 파일이름은 Dockerfile)
```
