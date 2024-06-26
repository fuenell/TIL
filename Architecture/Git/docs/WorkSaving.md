# 작업 저장
Add -> Commit -> Push 순으로 작업이 진행된다.

## Add
``` c#
// 모든 파일을 stage로 add한다
git add *
```

## Commit
``` c#
// 현재 stage에 존재하는 파일들의 상태를 하나의 커밋으로 묶는다
git commit -m "[커밋 메시지]"
```

## Push
``` c#
// 현재 stage에 존재하는 파일들의 상태를 하나의 커밋으로 묶는다
git push [옵션] [remote] [브런치명]
git push [옵션] [remote] [로컬 브런치명:원격 브런치명]  // 로컬 브런치와 원격 브런치의 이름이 다를 경우 사용

git push -u origin master
git push -f origin master:develop   // master의 내용을 develop 브런치에 덮어쓰기
```

**옵션**  
[-u]: 원격 브런치가 존재하지 않으면 생성 후 push한다  
[-f]: 현재 브런치를 덮어쓴다  
[-v]: 로그 출력  
