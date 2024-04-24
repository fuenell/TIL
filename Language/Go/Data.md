# 자료형
Go는 기본적으로 아래와 같은 내장 타입을 가지고 있다.
```
bool, int, float64, string
```

모든 변수는 선언 시 자동으로 기본값으로 초기화 된다.

### 변수 선언
```go
var flag1 bool
var flag2 bool = true
var flag3 = false
flag 4 := true
```
```
1. var [변수명] [자료형]
2. var [변수명] [자료형] = [값]
3. var [변수명] = [값]  // 값에 따라 자료형 자동 매칭
4. [변수명] := [값]  // 값에 따라 자료형 자동 매칭
```
정수형 기본형은 int이고 (운영체제 bit에 따라 크기가 변경됨)

실수형 기본형은 float64이다


### 형변환
Go는 자료형에 민감하기 때문에 자동 형변환을 지원하지 않는다.

int와 float의 연산도 타입 에러를 발생시킨다 `자료형(값)` 으로 명시적 형변환이 가능하다.

```go
var x int = 10
var y float64 = 3.14

var a float64 = float64(x) + y
var b int = x + int(y)
```

### 상수
var 대신 const를 붙여서 상수를 선언할 수 있다.

자료형을 지정해두지 않으면 int와 float에 모두 넣을 수 있지만 자료형을 지정해두면 해당 자료형으로만 사용할 수 있다.
```go
const x = 10
const y float64 = 3.14
```

### 배열

### 슬라이스

### 맵
