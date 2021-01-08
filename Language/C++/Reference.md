# 참조자 (Reference)
변수나 상수를 가리키는 기능  
포인터와 유사하나 약간의 편의성이 추가됨

## 특징
- 같은 변수를 2개의 이름으로 사용한다고 생각하는 것이 편함
- 선언과 동시에 초기화를 해주어야 함 (이후 변경 불가)
- 참조자 배열 선언 불가 (int &ref_arr[10])
- 리터럴을 참조하려면 const 로 참조자를 선언해야한다. (리터럴의 값을 바꿀 수 없기 때문에)

## 예제
### 기본 사용법
``` C++
int a = 0;
int &ref = a;    // 참조자 ref에 a 할당
ref = 1;         // a의 값 1로 변경
```

### 배열 참조자
``` C++
int arr[3] = {1, 2, 3};
int (&ref)[3] = arr;
ref[0] = 0;
```

### 매개변수
``` C++
void func1(int& ref) {
  ref = 1;    //a의 값이 1로 변경
}

int main() {
  int a = 0;
  function(a);
}
```

### 참조자 반환
``` C++
int a = 1;

int& func2(){
  return a;
}

int main(){
  int& ref = func2();    // 참조자 ref에 a 할당
}
```

### 매개변수 참조자 반환
``` C++
int& func3(int& ref) {
  return ref;
}

void main() {
  int a = 0;
  int &b = func3(a);    // 참조자 b에 a 할당
}
```
