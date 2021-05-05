# 배열/리스트 (Array, List)

선형 자료구조는 데이터를 순차적으로 나열된 형태로 가지는 구조를 말한다

대표적으로 배열, 리스트 등이 있다

## Array

배열은 고정된 크기를 가지는 자료구조이다

배열의 각 원소들은 실제 메모리상에 연속적으로 존재하는 특징이 있다

- 임의의 위치 원소 접근 (`[]`) : `O(1)`
- 모든 원소를 순차 탐색 : `O(n)`

```c#
int[] arr1 = { 0, 1, 2 };	// 크기가 3인 배열로 초기화
int[] arr2 = new int[3];    // 모든 원소 defualt 값인 0으로 초기화

arr2[0] = 5;

for (int i = 0; i < arr1.Length; i++)
{
    Console.WriteLine(arr1[i]);
    Console.WriteLine(arr2[i]);
}
```

## List

`List`는 제네릭으로 원소의 데이터 타입을 설정할 수 있는 가변길이 배열이다

내부적으로는 배열 확장이 필요하다면 2배 크기의 새로운 배열을 생성해 값을 복사한다

- 임의의 위치 원소 접근 (`[]`) : `O(1)`
- 검색 (`Contains`) : `O(n)`
- 가장 뒤 원소 추가 (`Add`) : `O(1)`
- 임의 위치 삽입, 삭제 (`Insert, RemoveAt`) : `O(n)`

``` c#
List<string> list = new List<string>();
list.Add("가");
list.Add("나");
list.Add("다");

list.Insert(1, "일");
list.RemoveAt(3);

if (list.Contains("나"))
    Console.WriteLine("Have[나]");

for (int i = 0; i < list.Count; i++)
    Console.WriteLine(list[i]);
```

#### ArrayList

`ArrayList`는 `object` 타입을 원소로 가지는 `List`이다

```c#
ArrayList arrList = new ArrayList();
arrList.Add(90);
arrList.Add(3.14f);
arrList.Add("str");

for (int i = 0; i < arrList.Count; i++)
    Console.WriteLine(arrList[i]);
```

## SortedList

`SortedList`은 `key`와 `value`값이 있는 가변길이 배열이다

`key`값으로 원소를 구분하기 때문에 `key`값은 중복될 수 없다

또한 원소 삽입 시  `key`를 기준으로 자동 정렬된다

- 임의의 위치 원소 접근 (`[]`) : `O(log n)`
- 원소 추가 (`Add`) : `O(n)`

- 검색 (`[]`) : `O(log n)`

```c#
SortedList<int, string> list = new SortedList<int, string>();
list.Add(1001, "Tim");
list.Add(1020, "Ted");
list.Add(1010, "Kim");

list[1013] = "New";    // 원소 삽입
list[1013] = "Old";    // 원소 수정

foreach (KeyValuePair<int, string> kv in list)
	Console.WriteLine("{0}:{1}", kv.Key, kv.Value);
```

## LinkedList

`LinkedList`는 링크 노드를 사용한 선형 자료구조이다

임의 위치의 원소를 참조하려면 `O(n)`이 소요되지만 삽입과 삭제가 `O(1)`이라는 장점이 있다

- 검색 (`Find`) : `O(n)`

- 삽입, 삭제 (`Add+위치, Remove`) : `O(1)`

``` c#
LinkedList<string> list = new LinkedList<string>();
list.AddFirst("Apple");
list.AddLast("Banana");

LinkedListNode<string> node = list.Find("Banana");
LinkedListNode<string> newNode = new LinkedListNode<string>("Grape");

// 새 노드를 Banana 노드 앞과 뒤에 추가
list.AddBefore(node, new LinkedListNode<string>("Grape"));
list.AddAfter(node, newNode);

list.Remove(node);

foreach (var item in list)
    Console.WriteLine(item);
```