# 구조체 (Struct)
구조체는 구조화된 데이터를 정의하기 위해서 사용한다

C++에서는 한 가지 특징을 제외하면 클래스와 거의 같은 문법이라고 볼 수 있다

## 선언 방법
간단히 클래스 문법에서 `class`대신 `struct`로 교체해 주면 된다

``` C++
struct Vector2 {
  int x;
  int y;
};  // 세미콜론 잊지 말자!

int main() {
  Vector2 vecotr2{0, 0};  // Vector2 구조체 초기화 생성자
}
```
### 접근 지시자
클래스에서는 기본 접근 지시자가 `private`이었지만, 구조체에서는 `public`이 기본 접근 지시자이다

이 부분이 클래스와 가장 큰 차이점이다

때문에 자료형 변수의 집합으로 구조체를 자주 사용한다

``` C++
struct Vector2 {
  int x;
  int y;
};

int main() {
  Vector2 vecotr2;
  vector2.x = 0;
  vector2.y = 0;
}
```
### 생성자 함수

클래스와 마찬가지로 생성자, 함수를 사용할 수 있다

``` C++
struct Vector2 {
  ...
  Vector2(int x, int y) : x(x), y(y) { }
    
  void sum(Vector2 vec2) {
    x += vec2.x;
    y += vec2.y;
  }
};
```
