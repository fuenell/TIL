# gitignore 재적용 (캐시 삭제)

gitignore를 수정해도 기존에 추가된 파일은 제외되지 않는다.

그때 아래 명령어를 실행하면 기존 캐시된 파일을 정리하기 때문에 gitignore가 다시 적용된다.

```
git rm -r --cached .
git add .
git commit -m "remove cached"
```


## 원리
git에서 한번 인덱스에 추가된 파일은 gitignore에 추가되더라도 그 파일을 계속 추적한다.

그때 `git rm -r --cached .`를 실행하면 인덱스에 있는 모든 파일을 제거한다.

그리고 `git add .`으로 모든 파일을 다시 인덱스에 추가한다.

이때는 삭제된 파일을 다시 추가하는 작업이기 때문에 gitignore를 적용받는다.
