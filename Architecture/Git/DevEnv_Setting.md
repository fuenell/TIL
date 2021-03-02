# 개발환경 설정

## 로컬 저장소 -> 원격 저장소
### Init
git 폴더 기본 설정
``` C#
// git 폴더 생성
git init
```

### Remote
git 주소 설정
``` C#
// git 연결된 주소 지정
git remote add [origin] [http:저장소.git]

// 연결된 주소 확인
git remote -v
```

## 원격 저장소 -> 로컬 저장소
### Clone
원격 Repository 복사
``` C#
// git 원격 저장소 복사
git clone [http:저장소.git]
```
