# 연산자 (Operator)
C++에서는 다음과 같이 클래스의 연산자를 오버로딩 할 수 있다  
`(리턴 타입) operator(연산자) (연산자가 받는 인자)`  
이때 통용적으로 사용되는 타입을 반환해주는 것이 좋다 (ex. `==` 는 `bool` 형식으로)

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
이때는 클래스 외부에서 함수를 오버로드 해야한다  
``` C++
A operator+(const A& a, const A& b) {
  return a.num + b.num;
}
```
a+b는  `*a.operator+(b);`와 `*operator+(a, b);`로 해석된다
때문에 둘 중 하나를 선언해야 컴파일러가 헛갈리지 않는다  
그리고 외부 이항 연산자는 연산 순서와 상관없이 작동하기 때문에  
아래와 같은 상황을 모두 수행하기 위해서는 외부 연산자 함수를 정의해야 한다
``` C++
a + 1   // a.operator+(A(1))와 operator+(A(1)+A(a)) 둘다 작동
1 + a   // operator+(A(1)+A(a))만 작동
```
## 출력 연산자
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

## 첨자 연산자
첨자 연산자는 `[]`으로 배열의 특정 원소에 접근할 때 사용한다  
때문에 배열 형식의 클래스에서 오버로딩한다면 편리하다
``` C++
class A {
  char& MyString::operator[](const int index) {   // char& 형식으로 반환해 str[10] = 'A' 가 가능하다
    return string_content[index]; 
  }
};
```

## 타입 변환 연산자
만약 `int`변수를 하나 담고있는 `int Wrapper`클래스가 필요하다  
이때 일반적인 `int`과 완전히 동일하게 작동하기 원한다면 클래스 내부에 타입 변환 연산자를 정의해주면 된다
``` C++
class Int {
  int data;
public:
  Int(int data) : data(data) {}
  Int(const Int& i) : data(i.data) {}

  operator int() { return data; }   // 타입 변환 연산자
};
int main() {
  Int x = 3;
  int a = x + 4;

  x = a * 2 + x + 4;
  std::cout << x << std::endl;
}
```

## 증감 연산자
전위 연산자와 후위 연산자를 구분하는 방법은 매개변수로 int 값을 넣어주는 것이다  
이때 일반적인 전위, 후위 연산자와 같이 작동하길 원한다면  
**전위 연산자는 값이 바뀐 자기 자신**을 반환하고,  
**후위 증감의 경우 값이 바뀌기 이전의 객체**를 반환해야 한다
``` C++
// 전위 연산자
A& operator++() {
  // A ++ 을 수행한다.
  return *this;
}

// 후위 연산자
A operator++(int) {
  A temp(A);
  // A++ 을 수행한다.
  return temp;
}
```
