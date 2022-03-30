# 가변 인수 (params) [#Docs](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/params)
배열로 전달받는 매개변수를 조금 더 쉽게 전달할 수 있다.

### 사용법
아래와 같이 `params` 키워드 뒤에 1차원 배열을 매개변수로 넣어 사용할 수 있다.  
호출할 때는 `1, 2, 3, ...` 이런 식으로 인수를 개수 제한 없이 전달할 수 있다.
``` c#
void Start()
{
    ParamsFunc(1, 2, 3, 4, 5);
}

void ParamsFunc(params int[] numbers)
{
    foreach (int num in numbers)
    {
        print(num);
    }
}
```

### 설명
아마 내부적으로 함수가 호출될 때 `numbers = new int[] {1, 2, 3, 4, 5};` 와 같은 코드가 적용되는 것 같다.  

그리고 구조상 구분이 불가능하기 때문에 `parmas` 키워드를 사용한 매개변수 뒤에 추가로 다른 매개변수를 사용할 수 없다.
