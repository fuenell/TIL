# 로컬 Git 동작 원리
다음 강의를 시청하고 동작 원리를 중심으로 내용 정리  
https://youtube.com/playlist?list=PLuHgQVnccGMA8iwZwrGyNXCGy2LAAsTXk


# .git 폴더
git의 모든 정보는 프로젝트에 있는 숨김 폴더인 .git에 저장된다.  
아래는 .git 폴더 속 중요한 파일들의 설명이다.

---

### index
add되어 스테이지에 올라간 모든 파일의 내용을 저장한다.
```
[파일 타입] [파일 내용 SHA1 해시값] [충돌 여부] [파일 경로]
100644 86b48d0e8c19c095a1a521cf2d59ad38ab1b3c9e 0 text.txt
```
`$ git ls-files --stage` 로 확인 가능하다.

---

### HEAD
현재 브런치를 가리킨다.
```
ref: refs/heads/[브런치 명]
```
`$ cat .git/HEAD` 로 확인 가능하다.

---

### refs/heads/[브런치 명]
해당 브런치의 최근 커밋을 가리킨다.
```
[커밋의 SHA1 해시값]
37109437345557a7ca934f2f4ec884604ddd13cb
```
`$ cat .git/refs/heads/[브런치 명]` 로 확인 가능하다.

---

## objects 폴더
3가지 형식의 파일이 저장되는 위치이다.  
파일 내용과 몇가지 내용을 합친 문자열을 SHA1을 사용해 해시값을 얻어내고,  
앞 2글자는 폴더로 나머지 글자는 파일 명으로 사용해 objects 폴더에 저장한다.  
`$ git cat-file -p [SHA1 해시값]` 으로 파일 내용을 확인할 수 있다.

### [objects/commit] commit 파일
커밋이 가리키는 tree와 이전 커밋이 저장된다.	(커밋의 상태 tree, 이전 commit, 작성자, 날짜, 커밋메세지)
```
tree [tree SHA1 해시값]		// 현재 커밋의 상태 
parent [commit SHA1 해시값]	// 이전 commit
[작성자, 시간]

[커밋 메시지]
```

### [objects/tree] tree 파일
index와 유사한 형태의 파일로 파일의 상태를 가지고 있다.	(파일 상태의 스냅샷)
```
040000 tree [SHA1 주소] [폴더명]
100644 blob [SHA1 주소] [파일명]
100644 blob [SHA1 주소] [파일명]
```
index와 다른 점은 폴더 속 파일은 tree로 중첩 구조로 표시된다. (github에서 폴더별 커밋 메세지가 표시되는 이유)

### [object/blob] blob 파일
파일의 내용을 가진다
```
[content]
```

1. 파일 add 시
파일 내용은 objects 폴더에 blob으로 저장됨
index에 blob의 해시값과 파일 명이 추가됨


3. 커밋 시
현재 index에 있는 내용을 tree로 만들고 그 tree와 최근 커밋을 가리키는 commit 생성
커밋의 내용(현재 커밋(tree), 이전 커밋,(parent) 작성자, 일시, 커밋메시지)이 objects/[SHA1] 위치로 저장된다


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
