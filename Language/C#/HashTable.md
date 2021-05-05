# 해시 테이블 (Hash Table)

해시 테이블은 `[key] - [value]` 구조를 가지고, 키값을 해시 함수를 통해 해싱하여 해시 테이블의 특정 위치로 직접 접근하는 방법이다

때문에 검색이 `O(1)`라는 엄청난 장점을 가진다

## Hashtable

`Hashtable`는 `object` 타입으로 키와 값을 저장하는 해시 테이블 자료구조이다

- 참조 (`[]`) : `O(1)`
- 삽입, 삭제 (`Add, Remove `) : `O(1)`
- 검색 (`Contains`) : `O(1)`

``` c#
Hashtable ht = new Hashtable();

ht.Add("PI", 3.14f);
ht['1'] = 1;    // 없는 키 참조 시 자동 삽입

ht.Remove("PI");

if (ht.Contains("PI"))
    Console.WriteLine(ht["PI"]);

foreach (DictionaryEntry item in ht)
    Console.WriteLine(item.Key + " : " + item.Value);
```

## Dictionary

`Dictionary`은 제네릭으로 키와 값의 데이터 타입을 설정할 수 있는 해시 테이블 자료구조이다

- 참조 (`[]`) : `O(1)`
- 삽입, 삭제 (`Add, Remove `) : `O(1)`
- 검색 (`ContainsKey`) : `O(1)`

``` c#
Dictionary<int, string> dict = new Dictionary<int, string>();

dict.Add(1, "일");
dict[2] = "이";    // 없는 키 참조 시 자동 삽입

dict.Remove(1);

if (dict.ContainsKey(2))
    Console.WriteLine(dict[2]);

foreach (KeyValuePair<int, string> item in dict)
    Console.WriteLine(item.Key + " : " + item.Value);
```