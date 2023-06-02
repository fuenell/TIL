# 구조체와 클래스의 차이 (값 형식, 참조 형식) [#Docs](https://learn.microsoft.com/ko-kr/dotnet/csharp/language-reference/language-specification/types#83-value-types)
구조체`struct` 와 클래스`class` 는 모두 변수, 함수 선언이 가능하기 때문에 큰 차이가 없는 것 처럼 보일 수 있다.  
하지만 구조체는 값 형식이고 클래스는 참조 형식으로 내부적인 처리 방식이 다르기 때문에 구분해서 사용하는 것이 좋다.  

# 값 형식
값 형식은 변수를 복사 시 해당 값이 복사되며 메모리에 값이 2개가 존재하게 된다.
값 형식은 struct, enum, 숫자 기본자료형 등이 있다.
``` C#
int a = 10; // a라는 변수에 10을 넣는다
int b = a;  // b라는 변수에 a와 같은 값인 10을 넣는다
b = 20;     // a를 넣은 b를 수정해도 b만 값이 변경된다
```

# 참조 형식
참조 형식은 변수를 복사 시 해당 값을 가리키는 참조자(포인터 개념)만 복사되기 때문에 실제 데이터는 메모리에 1개만 존재하게 된다.
참조 형식은 class, interface, array, delegate 등이 있다.
``` C#
List<int> a = new List<int>();  // a라는 List<int>를 선언한다
List<int> b = a;                // b에 a를 복사한다
b.Add(0);                       // b에 0을 추가하면 a와 b는 모두 0을 가지게 된다
```
