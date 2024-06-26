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
