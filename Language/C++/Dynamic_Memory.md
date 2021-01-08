# 동적 메모리 (Dynamic Memory)
new와 delete 키워드를 사용해서 힙영역의 메모리를 동적 할당할 수 있다.

## 특징
- 할당한 메모리를 해제 해주지 않는다면 메모리 누수가 발생할 수 있다.
- `new T – delete` 가 짝을 이루고 `new T[] - delete[]` 가 짝을 이룬다

## 예제
### 기본 사용법
``` C++
int* p = new int;    // 메모리를 동적으로 할당받아 포인터로 가리킨다
delete p;            // 할당받은 메모리를 해제시킨다
```

### 배열
``` C++
cin >> n;
int* list = new int[n];    // new로 크기가 동적인 배열을 정의 할 수 있다
int list2[n];              // 컴파일 에러 스택 영역은 컴파일 시 크기가 확정되어야 함

delete[] list;             // 동적 배열은 delete[]로 메모리를 해제할 있다
```
