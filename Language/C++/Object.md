# 객체 (Object)
객체란, 변수들과 참고 자료들로 이루어진 소프트웨어 덩어리다  
이 때 객체가 현실 세계에서의 존재하는 것들을 나타내기 위해서는 추상화(abstraction) 라는 과정이 필요하다  
`ex) 강아지가 짖는다 -> 강아지라는 객체에 짖다 라는 메소드로 추상화`

## 클래스 (Class)
C++에서 객체를 만들기 위해서는 클래스라는 설계도가 필요하다  
이후 클래스형의 변수를 만들어 클래스의 인스턴스를 만든다
``` C++
#include <iostream>

class Animal {
  int food;
  int weight;
};  // 세미콜론 잊지 말자!

int main() {
  Animal animal;  // Animal 클래스의 인스턴스 animal 생성
}
```
### 접근 지시자
`허용 범위:` 으로 밑에 있는 변수, 메소드의 접근 허용 범위를 설정할 수 있다 
``` C++
class Animal {
private:
  int food;
  int weight;

public:
  void increase_food(int inc) {
    food += inc;
    weight += (inc / 3);
  }
};
```
이런 식으로 접근 지시자로 인스턴스 변수를 직접적으로 바꿀 수 없게 캡슐화할 수 있다  
인스턴스 변수를 인스턴스 메소드를 통해서 간접적으로 조절하는 것을 캡슐화(Encapsulation) 라고 부른다
#### 캡슐화의 필요성
``` C++
Animal animal;
// 초기화 과정 생략

animal.food += 100;         // 불편 - food가 100증가하지만 이때 연계적으로 수정되어야 하는 값이 있을 수도 있다

animal.increase_food(100);  /* 편안 - food가 증가한다는 것을 메소드명으로 알 수 있고 
내부적으로 몸무게가 늘어난다와 같은 변경이 일어날 수도 있지만 외부에서는 신경쓰지 않아도 된다 */
```

### 생성자
``` C++
class Animal {
private:
  int food;
  int weight;

public:
  Animal(){}    // 기본 생성자 (생성자가 하나도 없으면 자동 생성)
  
  Animal(int f, int w){   // 생성자
    food = f;
    weight = w;
  }
};

int main(){
  Animal a1;            // 묵시적 기본 생정자 호출
  Animal a2 = Animal(); // 명시적 기본 생성자 호출
  
  Animal a3(10, 20);    // 생성자 호출
}
```
