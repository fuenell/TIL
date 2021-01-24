# 연산자 (Operator)
C++에서는 다음과 같이 클래스의 연산자를 오버로딩 할 수 있다  
`(리턴 타입) operator(연산자) (연산자가 받는 인자)`  

## 예문
일반적으로 연산자는 해당 클래스 내부에서 정의하고 외부에서 구현한다
``` C++
class A {
  int num;
  
  A operator+(const A& a) const {
    A temp(num + a.num);
    return temp;
  }
};
```

## 이항 연산자
이항 연산자인 `+, -, *, /, ->, =`는 매개변수를 2개 사용해서 연산자를 오버로딩할 수도 있다  
컴파일러는 매개변수가 1개인 연산자 함수를 우선 순위에 둔다
이때는 클래스 밖에서 함수를 오버로드 해야한다  
``` C++
A operator+(const A& a, const A& b) {
  return a.num + b.num;
}
```
또한 이미 `A operator+(const A&)`가 선언 되어 있을 경우 아래와 같이 사용할 수 있다  
이때 `a+b`를 사용할 수 없는 이유는 `a.operator+(b)` 와 `a+b`는 같은 의미지만  
컴파일러의 선택지가  `A operator+(const A&)`와 `A operator+(const A& a, const A& b)` 2개이기 때문이다
``` C++
A operator+(const A& a, const A& b) {
  return a.operator+(b);
}
```

## ostream
`std::cout` 에서 사용하는 `<<` 연산자 역시 오버로딩할 수 있다  
이때는 반환형을 `std::ostream&`형식으로 해야 `std::cout`에서 정상 동작한다
``` C++
std::ostream& operator<<(std::ostream& os, const A& a) {
  os << "A:(" << a.num << ")";
  return os;
}
```
`operator<<`는 클래스 밖에서 선언해야 해서 클래스의 `private`요소를 사용할 수 없다  
이때에는 get형식을 함수를 사용하거나 `friend`키워드를 사용할 수 있다  
``` C++
class A {
  friend std::ostream& operator<<(std::ostream& os, const A& a);
};
```
