# Photoshop, Figma에서 2D 리소스를 받을 때 주의사항

## 색공간 문제
Photoshop과 Figma는 Gamma 색공간을 사용하지만,  
하지만 유니티는 기본적으로 Linear 색공간을 사용한다.

두 색공간은 단색은 동일하게 보이지만 (투명한 이미지 사용 시) 색이 혼합될 시 다른 느낌이 난다

3가지 해결 방법
|방법|특징|
|-|-|
|툴을 Linear 색 공간에 맞추기|Photoshop은 비슷하게라도 설정이 가능하지만, Figma는 불가능|
|유니티를 Gamma로 설정해서 사용|3D 오브젝트의 색감이 모두 바뀜 (비추천)|
|불투명하게 받아서 눈대중으로 투명도 맞추기|작업 시간이 오래 걸림|

### Photoshop에서 Linear 색 공간 설정 방법
Edit > Color Settings에서 Blend RGB Colors Using Gamma 를 체크

## 외곽선 문제
폰트 외곽선이 inside, center, outside 중 어떤 종류인지 확인해야한다.  
TextMeshrPro에서는 설정하여 외곽선 종류를 맞출 수 있다. [설정 방법](./TextMeshProOutline.md#%EC%99%B8%EA%B3%BD%EC%84%A0-%EC%A4%91%EC%8B%AC%EC%A0%90)

또한 Figma에서는 도형의 크기를 확인할 때 외곽선이 있어도 외곽선 크기를 제외한 원래 도형을 크기를 알려주기 때문에 고려해서 크기를 조정해야한다. (외곽선을 inside로 설정하면 해결)

# 중심점
그림자가 같이 붙어있는 UI 리소스를 받으면 이미지를 중앙 정렬해도 중앙이 맞지 않는다.

3가지 해결 방법
|방법|특징|
|-|-|
|그림자의 크기만큼 반대편에 빈 공간을 추가해서 중앙을 맞춘다|리소스 크기가 늘어난다|
|Import Settings에서 Custom Pivot 설정|Sprite에서만 적용되고 UI에서는 적용되지 않는다.|
|눈대중으로 살짝 내려서 맞춘다|귀찮다|