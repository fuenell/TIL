# 브런치 설정

## 브런치 생성
``` c#
// 브런치 생성
git branch [branch]
// 해당 브런치로 이동
git checkout [branch]

// 브런치 생성 및 이동
git checkout -b [branch]
```

## 브런치 목록 확인
``` c#
// 원격 저장소 브런치 업데이트
git remote update

// 브런치 목록 출력
git branch [타겟] [옵션]
```
#### 타겟
[-l] : 로컬 브런치 (기본)  
[-r] : 원결 브런치  
[-a] : 모든 브런치  

#### 옵션
[-v] 최신 커밋 확인


## 브런치 삭제
``` c#
// 브런치 삭제
git branch -d [branch]
```
