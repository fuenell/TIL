# 미분 (Differential)
순간 변화율을 계산하는 것

## 평균 변화율

함수에서 x가 a에서 b로 변할 때 y의 변화량을 x의 변화량을 나눈 값을 평균 변화율이라고 한다.

ex) 『 $y = x^2$ 』 함수에서 x가 1부터 2사이일 때 y의 평균 변화율은 다음과 같이 계산된다.

|$$평균 변화율 = \frac{y변화량}{x변화량} = \frac{f(2) - f(1)}{2 - 1} = \frac{4 - 1}{1} = 3$$|
|-|

## 미분계수 (순간 변화율)

x의 값이 a인 순간에 y의 변화율은 다음과 같이 계산할 수 있다.

![image](https://user-images.githubusercontent.com/37904040/138879412-cfe2e33c-341c-4d05-9ddb-1b863e7820e4.png)

이를 미분계수(순간 변화율)라고 한다.

a가 1일 때 『 $y = x^2$ 』 함수의 미분계수는 다음과 같이 계산할 수 있다.

|$$\lim_{△x \to 0}\frac{f(1 + △x) - f(1)}{△x} = \frac{1 + 2△x + △x^2 - 1}{△x} = \frac{△x(2 + △x)}{△x} = 2 + △x = 2$$|
|-|

## 도함수

x가 정해지지 않고 미분계수를 함숫값으로 가지는 함수

다음과 같이 계산할 수 있다.

![image](https://user-images.githubusercontent.com/37904040/138890912-52538957-8fc5-4a15-bd03-b1ee808faf78.png)

도함수는 다음과 같이 표기하기도 한다.  
(y를 x로 미분했다, 함수 f를 x로 미분했다)
|$$y' = f'(x) = \frac{dy}{dx} = \frac{df}{dx}$$|
|-|

『 $y = x^2$ 』 함수의 도함수 다음과 같이 계산할 수 있다.

|$$\lim_{△x\to0} \frac{f(x+△x)-f(x)} {△x} = \frac{(x+△x)^2-x^2} {△x} = \frac{x^2+2△x^2+△x^2-x^2} {△x} = \frac{△x(2x+△x)} {△x} = 2x+△x = 2x$$|
|-|

## 미분의 기본성질 1

![image](https://user-images.githubusercontent.com/37904040/138892259-ea12f021-7386-4637-a4f2-87ff45251cfe.png)

예시) 『 $x^3 + 4x^2$ 』의 도함수 계산식

|$$\frac{d} {dx} (x^3+4x^2)$$|
|-|

공식 (2) 적용

|$$= \frac{d} {dx} (x^3) + \frac{d} {dx} (4x^2)$$|
|-|

공식 (1) 적용

|$$= \frac{d} {dx} (x^3) + 4\frac{d} {dx} (x^2)$$|
|-|

공식 (3) 적용

|$$= 3x^2+4*2x^1 = 3x^2+8x$$|
|-|

## 미분의 기본성질 2

![image](https://user-images.githubusercontent.com/37904040/138895133-cf7c72f5-2379-4b4d-8f92-378be5f472ab.png)

## 삼각함수의 도함수

![image](https://user-images.githubusercontent.com/37904040/139061215-e7ea70c8-faa6-47ee-84fc-e92a559970dd.png)
