# Sprite Edit

## Sprite Shape
스프라이트를 다양한 형태로 늘리고 구부릴 수 있다.

기본적으로 Repeat가 가능한 소스 이미지가 있어야 한다.

2D 프로젝트로 생성하고 씬에서 `2D Object > Sprite Shape` 으로 생성할 수 있다.  
종류는 Open과 Closed가 있다.

그리고 Sprite Shape Profile을 만들어서 텍스쳐를 설정할 수 있다.  
Texture에 Fill 이미지를 넣고, Sprites에 Egde 이미지를 넣는다.


### Open Sprite Shape
시작과 끝이 연결되어있지 않은 형태이다.  
선로나 로프를 표현하기게 적합하다.
Edge 이미지만 사용된다.

![image](https://github.com/user-attachments/assets/b9e1e451-7b04-4983-af0a-ee642756deb2)


### Closed Sprite Shape
시작과 끝이 연결된 형태이다.  
절벽이나 둘러쌓인 형태를 표현하기에 적합하다.
Profile 각도 조절로 일정 각도에서 Edge를 숨길 수 있다.

![image](https://github.com/user-attachments/assets/ec74114b-64d5-4a3f-8473-ae67ce26925f)



## Sprite Library
여러 Sprite 모여있는 오브젝트의 Sprite를 일부 교체 또는 전체 교체하는 기능이다.

### 사용법
먼저 `Assets > Create > 2D > Sprite Library Asset`으로 에셋 파일을 생성합니다.  
이후 `Open in Sprtite Library Editor`를 눌러서 카테고리와 라벨을 미리 만들어둔다.

[카테고리 - 라벨 예시]
|카테고리|라벨|
|-|-|
|눈|감은 눈, 뜬 눈|
|입|닫은 입, 벌린 입|
|손|주먹 쥔 손, 편 손|

상위 오브젝트에 `Sprite Library`를 추가하고  
Sprite를 사용하는 하위 오브젝트에 `Sprite Resolver`를 추가한 후 해당하는 카테고리를 설정한다.

이후 `Sprite Resolver`의 라벨을 변경해 Sprite의 상태를 변경할 수 있다.  
혹은 `Sprite Library`의 Assets을 미리 만들어둔 다른 에셋으로 교체해서 모든 스프라를 교체할 수도 있다.


![image](https://github.com/user-attachments/assets/0596e251-c22e-472e-8890-a43ede2f5628)
