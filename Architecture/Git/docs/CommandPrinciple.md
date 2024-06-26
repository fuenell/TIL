# GIT 명령어 동작 원리

## add 원리
파일 add 시 파일 내용은 objects 폴더에 blob으로 저장되고,  
index에 blob의 해시값과 경로가 추가된다.

## commit 원리
커밋 시 현재 index에 있는 내용을 tree로 만들고,  
그 tree와 이전 커밋을 가리키는 commit 파일 생성 후 브런치의 커밋을 해당 commit을 가리키게 한다.

## status 원리

실제 파일의 데이터와 index에 저장된 데이터 비교 -> add 전 (빨간색으로 표시)

가장 최근 commit의 tree파일과 index를 비교 -> add 후 (초록색 표시)
```
workspace -> index(stage) -> local rep -> remote rep
         add            commit        push
```
## branch 원리
HEAD: 현재 작업 중인 브런치를 가리킨다.
refs/heads/[브런치명]: 해당 브런치의 최신 커밋을 가리킨다.

브런치 생성 시 현재 커밋을 가리키는 브런치 refs/heads/[브런치명]에 생성한다.

## checkout 원리
HEAD가 이동할 브런치를 가리킨다.
실제 파일과 index는 해당 브런치가 가리키는 최신 커밋의 상태와 같아진다.

## reset 원리
soft: HEAD가 가리키는 브런치가 목표 커밋을 가리킴, index와 파일상태는 유지한다.

mixed: HEAD가 가리키는 브런치가 목표 커밋을 가리킴, index는 목표 커밋의 상태가 된다.

hard: HEAD가 가리키는 브런치가 목표 커밋을 가리킴, index는 목표 커밋의 상태가 됨, 파일의 상태도 돌아간 커밋의 상태가 된다.

## merge 원리
커밋 A에서 커밋 B를 병합한다면
커밋 A의 tree, 커밋 B의 tree, 공통 base 커밋의 tree로 3 way merge를 실행한다.  
3 way merge에서 충돌이 없으면 병합 커밋이 생성된다.

![image](https://user-images.githubusercontent.com/37904040/146739071-b1898138-caea-4c32-89bc-7bc53c233cf6.png)

### 충돌
3 way merge 중 자동 병합에 실패하면 파일 내용을 기호를 기준으로 나누고 충돌을 발생시킨다.  
index에 충돌된 파일이 3개가 출력된다 (번호가 붙는다)	// 3way merge  
```
1 -> 최근 공통 커밋 내용
2 -> 내가 수정한 내용
3 -> 병합할 내용
```

이후 충돌 해결 후 파일을 add를 하면 index에 파일이 하나만 남고 0번이 붙는다
