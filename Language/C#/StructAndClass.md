# 구조체와 클래스의 차이 [#Docs](https://learn.microsoft.com/ko-kr/dotnet/csharp/language-reference/language-specification/types#83-value-types)
구조체`struct` 와 클래스`class` 는 모두 변수, 함수 선언이 가능하다.  
때문에 큰 차이가 없는 것 처럼 보일 수 있지만, 내부적인 처리하는 형식이 다르기 때문에 구분해서 사용하는 것이 좋다.
구조체는 값 형식이고 클래스는 참조 형식이다.  

# 값 형식
값 형식은 변수를 복사 시 해당 값이 메모리에 2개가 존재하게 된다.
값 형식은 struct, enum, 숫자 기본자료형 등이 있다.
``` C#
int a = 10; // a라는 변수에 10을 넣는다
int b = a;  // b라는 변수에 a와 같은 값인 10을 넣는다
b = 20;     // a를 넣은 b를 수정해도 b만 값이 변경된다
```

# 참조 형식
참조 형식은 변수를 복사 시 해당 값을 가리키는 주소(포인터 개념)만 복사되기 때문에 실제 데이터는 메모리에 1개만 존재하게 된다.
참조 형식은 class, interface, array, delegate 등이 있다.
