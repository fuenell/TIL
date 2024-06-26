# yml [#Docs](https://docs.gitlab.com/ee/ci/yaml/index.html)
gitlab runner에서 Pipeline을 작성하는 파일

## 사용법
프로젝트 최상위에 `.gitlab-ci.yml` 파일을 만들어 작성하면 자동 적용

러너를 미리 설정해둬야 하지만 gitlab-runner(docker) 가 이미 설정되어있다고 가정

## 기본 문법
기본적으로 헤더 부분인 [단계 선언]과 내용인 [작업 선언] 2가지로 나뉜다.  
(띄어쓰기와 들여쓰기가 중요하다)
``` yml
stages:     # 파이프라인 단계 선언
  - build
  - deploy

android-build:          # 작업 명
  stage: build          # 작업의 단계
  image: ubuntu:22.04   # 작업이 실행될 도커 이미지
  script:               # 작업 실행 내용 작성
    - echo "빌드"

android-deploy:         # 작업 2
  stage: deploy
  image: ruby
  script:
    - echo "배포"
```

## 세부 설정
### 실행 조건
기본적으로 모든 push에 대하여 파이프라인 작업이 시작되지만,  
각 작업이 언제 시작될지 지정할 수 있다. (조건문을 다양하게 변경 가능하다)
``` yml
android-build:
  stage: build
  rules:
    - if: $CI_COMMIT_BRANCH == "master"   # master 브런치 push 시 실행
...
```

### 러너 태그
tags로 원하는 러너를 지정해서 작업을 실행할 수 있다.
``` yml
android-build:
  stage: build
  tags:
    - myrunner
...
```

### 변수
variables로 변수를 선언할 수 있다.  
변수는 `$변수명` 또는 `${변수명}`으로 다른 곳에서 사용할 수 있다.  
변수는 전역 변수, 지역 변수와 개념과 비슷해서 작업 안팎에서 선언할 수 있다.
``` yml
android-build:
  stage: build
  variables:
    MY_NUMMBER: 1
    MY_STRING: "A"
  script:
    - echo ${MY_NUMMBER} $MY_STRING
```

### 클론
각 작업 시작되면 자동으로 해당 프로젝트를 clone 받고 작업이 실행된다.  
하지만 예약된 변수인 `GIT_STRATEGY`를 수정해 변경할 수 있다.
``` yml
variables:
  GIT_STRATEGY: fetch # featch: 가장 최신 커밋만 받는다 / none: clone 받지 않는다
```

### 캐시
캐시 기능을 사용하면 작업 시 생성된 파일을 다음 작업에 다시 사용할 수 있다.  
작업 실행 전 전에 저장된 캐시가 있으면 불러오고 작업이 끝나고 다시 캐시를 저장한다.
``` yml
android-build:
  stage: build
  cache:
    key: "MyCacheKey"   # 캐시를 저장하고 불러올 때 사용할 키
    paths:              # 캐시할 파일의 경로
      - Library/
```

### 아티팩트
작업이 끝나고 특정 파일을 업로드 하는 기능이다.
``` yml
android-build:
  stage: build
  artifacts:
    paths:              # 업로드할 아티팩트 파일 경로
      - Builds/
    when: on_success    # 작업 성공 시에만 아티팩트 업로드
    expire_in: 30 days  # 아티팩트 만료기간
```

### 의존
특정 작업이 끝나고 실행되도록 설정하는 기능이다.  
의존하는 변수, 아티팩트를 가져온다.
``` yml
android-build:
  stage: build

android-deploy:
  stage: deploy
  dependencies: 
    - android-build     # android-build 작업이 완료되어야 실행
```