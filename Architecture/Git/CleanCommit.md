# Commit 기록 정리

## 문제점
프로젝트의 크기가 점점 커져(50GB) Commit 기록을 확인할 때마다 랙이 걸린다.

### 목표
많은 수의 커밋을 압축해 버전 별로 하나씩만 남긴다.

### 시도 1
일반적으로 커밋을 합칠 때에는 Squash를 사용하기 때문에 `git rebase -i`명령어로 500개의 커밋을 Squash 했다.  
그러나 500개의 커밋에서 발생했던 충돌을 모두 하나하나 해결해야 했다.  
모든 충돌을 병합되는 쪽을 사용하도록 선택해 Squash를 완료했지만 파일이 변경되어 취소하고 처음으로 돌아갔다.

### 시도 2
일반적인 Squash는 병합된 커밋을 제거하지만 `git rebase -i --rebase-merges`를 사용하면 병합된 커밋도 함께 Squash 할 수 있었다.  
그러나 마찬가지로 중간중간 충돌이 발생해 정확하게 충돌을 해결해 줘야 했다.

### 시도 3
그러다가 굳이 Squash를 사용해서 커밋을 줄일 필요가 없다는 것을 깨달은 후 `reset --hard`로 원하는 버전으로 이동한 뒤  
`reset --soft`로 첫 커밋으로 이동해 새로운 커밋을 남기고 이후 다음 버전의 파일 상태를 가져와서  
새로운 커밋을 추가하고를 반복해서 결국 기존에 500여개의 커밋을 10개로 압축했다.
![image](https://user-images.githubusercontent.com/37904040/140445473-c9c0e406-9ce8-436f-ba67-f1332fcb9144.png)

## 문제점 2
커밋을 압축하는 것에는 성공했지만 이것들을 강제로 Push 하니 기존 커밋들의 기록이 보이지 않는 곳에 남아서 원격 저장소의 용량이 더 늘어났다.

### 시도 2-1
`git reflog expire --expire=now --all && git gc --prune=now --aggressive`로 사용되지 않는 기록을 삭제하고 `git push origin -f --all`로 전체 강제 Push 했다.  
로컬에서는 .git폴더의 용량이 줄어들었지만 줄어든 정보가 원격 저장소에는 덮어쓰기가 되지 않는 것 같았다.

### 시도 2-2
GitLab 저장소 설정에 있는 [Housekeeping](https://docs.gitlab.com/ee/administration/housekeeping.html) 작업을 실행시켰다.  
Housekeeping는 연결되지 않은 기록을 정리하는 기능이었다.  
기본적으로는 자동으로 일정 주기마다 작동하는 것 같지만 수동으로 실행시킬 수 있었다.  
다음날 확인해보니 원격 저장소의 용량이 절반으로 줄어든 것을 확인할 수 있었다.

## 결론
develop과 master에서는 직접적으로 커밋하지 말고 feature branch에서 커밋 후  
squash & fast-forward 후 fature branch를 삭제해 커밋을 깔끔하게 남기는 방식을 채택해서 사용하기로 했다.
