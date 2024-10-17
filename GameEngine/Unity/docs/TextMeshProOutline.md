# TextMeshPro Outline
레거시 Text에는 외곽선을 지원하지 않지만 TextMeshPro에서는 외곽선을 지원한다.


## 외곽선 추가
외곽선을 추가하려면 TextMeshPro의 Material에서 Outline 속성을 활성화하면 된다.

그리고 Thickness 옵션으로 외곽선의 두께 비율(0 ~ 1)을 설정할 수 있다.

![image](https://github.com/fuenell/TIL/assets/37904040/795e2742-d9bd-4615-b414-bf09e3b89bd1)


## 외곽선 두께
외곽선의 두께는 Thickness와 폰트의 크기에 비례해 결정된다.

외곽선의 두께를 픽셀 단위로 조정하려면 아래 공식에 따라 Thickness를 조정하면 된다.
```
Thickness = (외곽선_픽셀 * 0.5) / (폰트_크기 * 0.1)

예를 들어 20pt의 텍스트의 외곽선을 2px 두께로 설정하려면 Thickness를 0.5로 맞추면 된다.

Thickness = (2 * 0.5) / (20 * 0.1) = 0.5
```
하지만 외곽선은 폰트 크기에 비례해 일정 두께 이상 더 두꺼워지지 않는다.


## 외곽선 중심점
외곽선을 그리는 방식은 크게 3종류가 있다.

![image](https://github.com/fuenell/TIL/assets/37904040/caee9952-aaa1-4479-9dfa-fb749fc3b63c)

그 중 TextMeshPro에서는 기본적으로 center 방식으로 외곽선을 그린다.

하지만 Face 속성에 Dilate 값을 조정해서 방식을 변경할 수 있다.
```
inside
> Dilate = Thickness * -1

center
> Dilate = 0

outside
> Dilate = Thickness
```

![그림1](https://github.com/fuenell/TIL/assets/37904040/232d7656-64be-4631-85ed-376951318b3f)


## 외곽선 색상
만약 프로젝트의 색 공간이 Linear일 경우 Outline의 HDR Color와 색 공간이 달라,  
같은 RGB 값을 가지더라도 다른 색을 표현할 수 있다.

해결 방법으로는 색 공간을 Gamma로 변경하거나, 원하는 색상을 스포이트로 클릭해 지정할 수 있다.
