# Transparency Sort Mode [#API](https://docs.unity3d.com/2020.3/Documentation/ScriptReference/TransparencySortMode.html)
Sorting 레이어가 같은 Sprite의 렌더 순서를 좌표값으로 결정하는 설정이다

defualt로 설정하면 Axis는 `0, 0, 1`가 되며 z좌표가 클 수록 뒤로 배치된다

이때 Custom Axis로 변경 후 Axis를 변경하면 다음과 같은 기준으로 렌더 순서가 정렬된다
```
// 렌더 순서가 작은 스프라이트가 앞으로 오도록 정렬
(x좌표 * x축) + (y좌표 * y축) + (z좌표 * z축) = 렌더 순서
```
ex) Axis를 `0, 1, 0`로 변경하면 Y축이 클 수록 뒤로 배치된다  

![TransparencySortMode](https://user-images.githubusercontent.com/37904040/129332847-244e8cd0-99c9-463c-abf7-2d3cae92f6af.jpg)
