# 색 공간 [#Manual](https://docs.unity3d.com/kr/2022.3/Manual/LinearRendering-LinearOrGammaWorkflow.html) [#Unity_Korea_Youtube](https://youtu.be/Xwlm5V-bnBc?si=3HPXD4KsaJl9NvRq)

카메라로 사진을 찍으면 사람의 눈의 보정이 없는 순수한 색상 값이 저장된다.

왜냐하면 사람은 어두운 구간의 색을 더 잘 구분하기 때문에 보정이 없는 순수한 색상은 실제보다 밝게 느껴진다.

그래서 그 차이를 보정하기 위해 모니터는 순수한 색상 값에서 살짝 어둡게 출력한다.



## 유니티 색 공간 설정
유니티에서 설정할 수 있는 색 공간은 `Linear`와 `Gamma`가 있다.

여기서 말하는 `Linear`와 `Gamma` 는 랜더 연산을 수행할 색 공간을 결정하는 것이다.

색 공간은 `Project Settings > Player > Other Settings > Color Space` 에서 설정할 수 있다.


`Gamma`로 설정할 경우 실제보다 밝게 저장된 이미지 끼리 랜더 연산을 수행한다.  
따라서 굉장히 밝게 나온다.

`Linear`로 설정할 경우 실제보다 밝게 저장된 이미지를 다시 어둡게 만든 뒤(선형 색 공간에서) 연산을 수행한다.  
선형 공간에서 연산을 수행해야 정상적인 결과가 나옵니다.

![image](https://github.com/user-attachments/assets/1c0cc29a-33b9-4d83-b3bb-82194c026d1e)


최근 기기들은 대부분 linear 연산 방식을 지원하기 때문에 웬만하면 linear를 사용하면 된다.


## sRGB
sRGB는 실제보다 밝게 저장되는 이미지를 뜻한다.  
대부분의 jpg, png 이미지 파일이 sRGB로 저장되어 있다.  
sRGB가 아닌 이미지가 있다면 Texture Import Settings에서 sRGB 옵션을 끄면 된다.

sRGB 이미지는 이미 밝게 저장되어 있기 때문에 단순히 그대로 출력하는 경우는 상관없지만,  
내부적으로 연산을 할 때를 위해 [이 텍스처를 sRGB로 해석할지]를 지정하는 옵션이 필요하다.


## 여담
기본값이 linear 라서 따로 수정할 필요는 없지만  
포토샵이 투명도 연산 방식이 Gamma 와 유사하기 때문에  
유니티를 gamma로 맞추거나 포토샵을 linear에 가깝게 설정할 수 있다.

빛을 사용하지 않는 2D의 경우 투명도가 들어간 이미지를 사용할 때를 제외하고 색공간이 영향을 주지 않는다

