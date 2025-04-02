# Unity Search [#Manual](https://docs.unity3d.com/kr/current/Manual/search-overview.html)
유니티 세부 검색 기능이다.

`Ctrl + K` 명령어를 입력하거나, 검색창 옆에 있는 버튼을 눌러서 창을 열 수 있다.

![Image](https://github.com/user-attachments/assets/8d78d2a7-8187-4908-8ada-ff857c1c2b4f)


## 주요 문법
|필터|예시|설명|
|-|-|-|
|필터 없이 검색|`gamemanager`|해당 내용이 포함되어(이름, 경로 등) 모든 것을 검색|
|name|`name:unit`|Project에서 파일 이름 검색|
|dir|`dir:"Assets/Graphics"`|Project에서 경로에 있는 파일들 검색 (유니티6 부터는 dir="..." 도 가능)|
|t|`t:prefab`|타입, 모든 컴포넌트 검색|
|vertices|`vertices>=100`|버텍스 수를 검색 (다른 부등호도 사용 가능)|
|인스펙터 변수|movespeed=5|인스펙터에 보이는 값으로 입력해야 한다 (띄어쓰기X)|
|#인스펙터 변수명|#m_movespeed=5|실제 변수명과 똑같이 검색해야 한다|
|enum검색|인스펙터변수=<$enum:Default,TextureImporterType$>|<$enum:값,타입$> 형식으로 특정 enum도 검색이 가능하다.|

```
띄어쓰기로 and 연산

모든 조건 앞에 '-' 를 붙이면 해당 조건을 제외한다는 의미가 된다
```

## 예시
"Assets/Graphics" 경로 속 버텍스가 100 이상이고 unit 이 포함되지 않은 모든 prefab을 검색
```
dir:"Assets/Graphics" vertices>=100 -name:unit t:prefab
```

## 쿼리 빌더
검색창 좌측의 토클 버튼으로 쿼리 빌더(GUI)모드로 전환이 가능하다.  
쿼리 빌더 모드에서는 우측의 `+` 버튼으로 조건을 추가 가능하다.  
각 조건은 우클릭 후 `Delete` 메뉴로 삭제가 가능하다.  
또한 `Exclude` 옵션을 설정해서 조건 반전이 가능하다.  

![Image](https://github.com/user-attachments/assets/3595392f-46c6-4a22-a0b1-6a34931ac823)

