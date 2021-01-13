# 정적 (Static)
## 정적 변수
멤버 변수에 static 키워드를 붙여 정적 멤버 변수로 사용할 수 있다
``` C++
class A {
public:
  static int total_count;
  A() {
    total_count++;
  }
  ~A() {
    total_count--;
  }
};

int A::total_count = 0;   // !중요! 정적 변수 초기화

int main() {
  A a1;
  A a2;
  
  cout << A::total_count;
}
```
## 정적 함수
멤버 함수에 static 키워드를 붙여 정적 멤버 함수로 사용할 수 있다  
정적 함수에서는 정적 변수만 사용 가능하다
``` C++
class A {
...
  static int total_count;
  
  static int get_total_count() {
    return total_count;
  }
};

int A::total_count = 0;   // !중요! 정적 변수 초기화

int main() {
  A a1;
  A a2;
  
  cout << A::get_total_count();
}
```
