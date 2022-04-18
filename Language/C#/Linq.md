# Linq
Linq는 C#에서 사용하는 쿼리문이다.  
`using System.Linq;` 를 추가해서 사용할 수 있다.

### Where
다음과 같이 where문으로 원하는 조건의 원소들을 한번에 찾아낼 수 있다.
``` C#
int[] intArray = { -2, -1, 0, 1, 2 };

var positiveNumbers = from n in intArray
                      where 0 < n
                      select n;
```
또는 다음과 같이 표현할 수도 있다.
``` C#
var positiveNumbers = intArray.Where(n => 0 < n);
```

### Range
자료형을 연속 데이터로 한번에 초기화할 수 있다.  
0~100으로 자료형으로 초기화하려면 다음과 같이 for문을 작성해야할 것이다.
``` C#
List<int> intList = new List<int>();
for(int i = 0; i < 100; i++)
{
  intList.Add(i);
}
```
하지만 Linq의 Range를 사용하면 다음과 같이 작성할 수 있다.
``` C#
List<int> intList = Enumerable.Range(0, 100).ToList();
```
### All/Any
특정 속성 존재 여부를 검사하는 함수이다.  
`All`은 모든 요소가 조건을 만족하면 `true`를 반환한다.  
`Any`는 요소 중 하나라도 조건을 만족하면 `true`를 반환한다. 
``` C#
int[] intArray = { -2, -1, 0, 1, 2 };

if(intArray.All(n => n < 0))
{
  print("모든 수는 음수입니다.");
}

if(intArray.Any(n => n < 0))
{
  print("음수가 존재합니다.");
}
```
