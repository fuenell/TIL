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
