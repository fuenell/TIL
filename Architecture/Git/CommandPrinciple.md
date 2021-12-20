# GIT 명령어 동작 원리

## add
1. 파일 add 시 파일 내용은 objects 폴더에 blob으로 저장됨
2. index에 blob의 해시값과 경로가 추가됨

## commit
3. 커밋 시 현재 index에 있는 내용을 tree로 만들고 그 tree와 이전 커밋을 가리키는 commit 파일 생성 후 브런치의 커밋을 해당 commit을 가리키게 함

## status 원리
index: 스테이지에 올라온 파일 상태를 저장 (tree와 유사한 형태)

실제 파일의 데이터와 index에 저장된 데이터 비교	-> add 전 (빨간색으로 표시)
가장 최근 commit의 tree파일과 index를 비교		-> add 후 (초록색 표시)
=========
workspace -> index(stage) -> local rep -> remote rep
	add	      commit	push

6. branch의 원리
HEAD: 현재 작업 중인 브런치를 가리킴
refs/heads/[브런치명]: 해당 브런치의 최신 커밋을 가리킴

브런치 생성 시 현재 커밋을 가리키는 브런치 refs/heads/[브런치명]에 생성

7. checkout의 원리
HEAD가 이동할 브런치를 가리킴
실제 파일과 index는 해당 브런치가 가리키는 최신 커밋의 상태과 같아짐

8. reset의 원리
soft: head가 가리키는 브런치가 돌아간 커밋을 가리킴, index과 파일상태는 유지
mixed: head가 가리키는 브런치가 돌아간 커밋을 가리킴, index는 돌아간 커밋의 상태가 됨
HARD: head가 가리키는 브런치가 돌아간 커밋을 가리킴, index는 돌아간 커밋의 상태가 됨, 파일의 상태도 돌아간 커밋의 상태가 됨

9. 병합의 원리
커밋 A와 커밋 B를 병합한다.
이 때 공통 base를 기준으로 3 way merge를 실행한다
3 way merge에서 충돌이 없으면 병합 커밋이 생성된다

9. 충돌의 원리
3 way merge 중 자동 병합에 실패하면 파일 내용을 기호를 기준으로 나누고 충돌을 발생시킨다.
index에 충돌된 파일이 3개가 출력된다 (번호가 붙는다)	// 3way merge
1 -> 최근 공통 커밋 내용
2 -> 내가 수정한 내용
3 -> 병합할 내용

이후 충돌 해결 후 파일을 add를 하면 index에 파일이 하나만 남고 0번이 붙는다

- [tree] 파일상태의 스냅샷 (index 파일 느낌)
=====
040000 tree [SHA1 주소] [폴더명]
100644 blob [SHA1 주소] [파일명]
100644 blob [SHA1 주소] [파일명]
=====

- [commit] 현재 커밋(tree), 이전 커밋,(parent) 작성자, 일시, 커밋메시지가 들어있는 파일
=====
tree [SHA1 주소]		// 현재 커밋한 파일 상태
parent [SHA1 주소]		// 이전 커밋의 파일 상태
[작성자+시간]

[커밋 메시지]
=====
5. status 원리
index: 스테이지에 올라온 파일 상태를 저장 (tree와 유사한 형태)

실제 파일의 데이터와 index에 저장된 데이터 비교	-> add 전 (빨간색으로 표시)
가장 최근 commit의 tree파일과 index를 비교		-> add 후 (초록색 표시)
=========
workspace -> index(stage) -> local rep -> remote rep
	add	      commit	push

6. branch의 원리
HEAD: 현재 작업 중인 브런치를 가리킴
refs/heads/[브런치명]: 해당 브런치의 최신 커밋을 가리킴

7. reset의 원리
SOFT: 현재 상태가 스테이지에 남아있, HEAD는 뒤로 간다
soft: head가 가리키는 브런치가 돌아간 커밋을 가리킴, index과 파일상태는 유지
mixed: head가 가리키는 브런치가 돌아간 커밋을 가리킴, index는 돌아간 커밋의 상태가 됨
HARD: head가 가리키는 브런치가 돌아간 커밋을 가리킴, index는 돌아간 커밋의 상태가 됨, 파일의 상태도 돌아간 커밋의 상태가 됨
