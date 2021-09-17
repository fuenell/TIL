# 로컬 Git 동작 원리
다음 강의를 시청하고 핵심 내용 정리  
https://youtube.com/playlist?list=PLuHgQVnccGMA8iwZwrGyNXCGy2LAAsTXk

## add
파일 add 시 다음과 같은 내용이 저장된다  
파일의 내용과 몇가지 정보를 SHA1 암호화 후 `objects/[SHA1 앞 2글자]/[SHA1 나머지 부분]` 에 저장된다
index에 해당 주소와 파일 명이 추가됨

2. 파일 내용 주소 지정 방식
파일 내용과 몇가지 내용을 합친 문자열을 SHA1을 사용해 해시값을 얻어내고
앞 2글자는 폴더로 나머지 글자는 파일 명으로 사용해 objects 폴더에 저장한다
```
index - [1234...] file1.txt

objects/12/34... - [내용]
```

3. 커밋 시
커밋의 내용(현재 커밋(tree), 이전 커밋,(parent) 작성자, 일시, 커밋메시지)이 objects/[SHA1] 위치로 저장된다

==[tree]==
[SHA1주소] file1.txt
[SHA1주소] file2.txt
=======

==[parent]==
[SHA1주소] file1.txt
=======

4. objects속 파일 유형
- [blob] 파일의 내용이 들어가 있는 파일
=====
a
=====

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
