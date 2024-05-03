# 구조체

### 구조체 타입 선언
아래와 같이 구조체 타입을 선언할 수 있다.
``` go
type person struct {
    name string
    age int
}

type [구조체명] struct {
    [변수명1] [자료형1]
    [변수명2] [자료형2]
}
```

### 구조체 변수 선언
타입 선언 이후에서는 아래와 같이 변수를 선언할 수 있다.  
구조체의 기본값은 구조체의 모든 항목이 기본값으로 설정된 상태이다.
``` go
var man1 person

man2 := person{}

// 구조체 항목을 순서대로 초기화한다
man3 := person{
    "man",
    20,
}

// 구조체 항목을 지정해서 초기화한다
man4 := person{
    age: 20,
    name: "man",
}
```

### 익명 구조체 선언
``` go
var man1 struct {
    name string
    age int
}

var man2 = struct {
    name string
    age int
}{
    name: "man",
    age: 20,
}
```

### 사용법
값 변경
비교
출력
