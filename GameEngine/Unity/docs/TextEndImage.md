# 텍스트 끝에 이미지 배치
늘어나는 텍스트 양 끝에 이미지를 정확하게 배치하는 방법


## 택스트 크기로 맞추기

1. 텍스트 박스 크기를 자동으로 조정

텍스트 컴포넌트에 있는 오브젝트에 `Content Size Fitter`을 추가하고 옵션을 `Preferred Size`으로 설정한다.

![Image](https://github.com/user-attachments/assets/256a43c5-40fe-4f2a-a965-ae44af97b92c)

2. 이미지를 텍스트 자식으로 넣어서 피봇을 맞춘다

Anchors의 X값을 1,1
Pivot의 X값을 0
Pos X값을 0

![Image](https://github.com/user-attachments/assets/269475fc-8dba-4160-8ecc-fdfe998b521e)


## 레이아웃 그룹 사용

레이아웃 그룹을 만들어서 `Cotrol Child Size`만 체크하고 텍스트와 이미지 오브젝트 순서대로 자식으로 넣으면 알아서 크기도 맞춰지면서 정렬됨 (텍스트와 이미지는 기본 설정으로)

![Image](https://github.com/user-attachments/assets/b5c95ac4-ccdd-4c19-a47a-9e5c4a90890c)


## tmp sprite로 글자 끝에 넣기
TMP 이모지 넣기 검색 ㄱ