# Unity Search [#Manual](https://docs.unity3d.com/kr/current/Manual/search-overview.html)
유니티 세부 검색 기능이다.

`Ctrl + K` 명령어를 입력하거나, 검색창 옆에 있는 버튼을 눌러서 창을 열 수 있다.

![Image](https://github.com/user-attachments/assets/8d78d2a7-8187-4908-8ada-ff857c1c2b4f)


## 주요 문법
띄어쓰기로 and 연산
```
dir:"Assets/Graphics"   // 유니티6 부터는 dir="..." 도 가능
t:prefab   // 타입, 컴포넌트

vertices>=100
movespeed=39    // 인스펙터에 표시되는 모든 값 검색 가능
그외 대부분의 인스펙터에 표시되는 값 검색 가능

texturetype=<$enum:Default,TextureImporterType$>  // <$enum:값,타입$> 형식으로 특정 enum도 검색이 가능하다.

모든 조건 앞에 '-' 를 붙이면 해당 조건을 제외한다는 의미가 된다
```

## 예시
"Assets/Graphics" 경로 속 버텍스가 100 이상이고 이름에 unit 이 들어가지 않은 모든 prefab을 검색
```
dir:"Assets/Graphics" vertices>=100 -unit t:prefab
```

## 쿼리 빌더
검색창 좌측의 토클 버튼으로 쿼리 빌더(GUI)모드로 전환이 가능하다.  
쿼리 빌더 모드에서는 우측의 `+` 버튼으로 조건을 추가 가능하다.  
각 조건은 우클릭 후 `Delete` 메뉴로 삭제가 가능하다.  
또한 `Exclude` 옵션을 설정해서 조건 반전이 가능하다.  

![Image](https://github.com/user-attachments/assets/3595392f-46c6-4a22-a0b1-6a34931ac823)

