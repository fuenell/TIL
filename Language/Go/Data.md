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
선언
```go
var arr [3]int
var arr = [3]int{10, 20, 30}
var arr = [10]int{10, 20, 30}            // 남은 공간은 0으로 초기화
var arr = [...]int{10, 20, 30}           // 크기를 ...으로 선언하면 리터럴 개수에 따라 자동으로 3칸이 됨
var arr = [10]int{2: 10, 5: 20, 8: 30}   // 2번 위치에 10, 5번 위치에 20, 8번 위치에 30으로 초기화
```

사용
```go
arr[1] = 5
print(arr[1])

print(arr1 == arr2)  // 같은 크기의 배열은 비교 가능, 모든 요소가 같으면 true

print(len(arr))  // 크기 출력
```

### 슬라이스
리스트와 유사한 자료형이다.

배열을 선언할 때 크기를 입력하지 않으면 슬라이스가 된다.
```go
var slice []int  // 슬라이스 선언

print(slice == nil)  // 초기화 하지 않는 슬라이스는 nil값을 가진다 (null과 유사), 또한 슬라이스 끼리 비교는 불가능하다

slice = make([]int, 10)  // 0이 10개 있는 슬라이스로 초기화 함

slice = append(slice, 1, 2, 3)  // 슬라이스에 1, 2, 3을 추가

print(len(slice), cap(slice))  // cap은 슬라이스가 확보한 메모리 크기를 확인할 수 있다

var slice1 = slice[0:5]  // 0부터 5위치 까지 영역을 복사함 (주소 복사)

var slice2 = make([]int, 3)
copy(slice2, slice)  // slice2의 크기 만큼 3칸까지 복사 (값 복사)

slice = append(slice[:i], slice[i+1:]...)  // i에 있는 요소를 삭제 (제외 후 재구성)
```

### 맵
```go
var map1 map[string]int  // map[KeyType]ValueType 으로 맵을 선언할 수 있다 (nil값을 가진다)

var map2 = map[string]int{  // 이렇게 선언과 동시에 초기화할 수도 있다
  "one":   1,
  "two":   2,
  "three": 3,
}

map2["four"] = 4  // key값이 없으면 추가된다, 있으면 수정

print(map2["three"])  // key에 해당하는 값 출력 (없으면 기본값)

var v, ok = map2["five"]  // ok에는 key 존재여부, v에는 값 반환 (없으면 기본값)

delete(map2, "one")  // key값에 해당하는 키와 값 삭제
```
