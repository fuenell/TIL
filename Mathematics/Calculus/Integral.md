# 적분 (Integral)
설명

## 부정적분

F'(x) = f(x) 일 때 F를 f의 부정적문이라고 한다.

부정적분은 ∫와 dx로 표기한다.

일반적으로 부정적분을 구할 때 적분한다 라는 말을 사용한다.

```
F'(x) = f(x) 일 때
∫f(x)dx = F(x)
```

### 부정적분 기본 공식

미분 공식의 반대이다.

![image](https://user-images.githubusercontent.com/37904040/144226934-a60b743f-f634-45d6-ab43-4e202de2b401.png)

![image](https://user-images.githubusercontent.com/37904040/144227689-7f7cb3c2-0525-4b90-a0c9-df7aba74bcac.png)

## 정적분

 x값이 a에서 b일 때 f(x) 함수와 x축사이의 넓이를 구하는 것

함수를 n등분하고 각 등분에서 밑변의 길이와 높이를 모두 합한 값

```
∫ab f(x)dx
```

![image](https://user-images.githubusercontent.com/37904040/144245949-8b3045d7-b60a-4d17-b809-c37d42193715.png) = ![image](https://user-images.githubusercontent.com/37904040/144246421-cc2cf8a4-a52d-4e5f-96cc-03a37407f5ab.png)

```
f(x) = x
이고 0에서 1의 정적분을 구하면
lim ∑ f(k(b-a)/n) * ((b-a)/n)
lim ∑ f(k/n) * (1/n)
lim ∑ (k/n) * (1/n)
lim ∑ k/n^2
lim 1/n^2 * ∑ k
lim 1/n^2 * n(n+1)/2
lim n(n+1)/2n^2
= 0.5
```

