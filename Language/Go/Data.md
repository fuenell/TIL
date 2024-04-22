# 자료형
Go는 기본적으로 아래와 같은 내장 타입을 가지고 있다
```
bool, int, float, string
```

모든 변수는 선언 시 자동으로 기본값으로 초기화 된다.

#### 변수 선언
```go
var flag bool
var flag = true

1. var [변수명] [자료형]
2. var [변수명] = [값]  // 값에 따라 자료형 자동 매칭
```

int 선언 시 64비트에서는 int64, 32비트에서는 int32가 된다.

float는 float32, float64가 있고 float64가 기본형이다
