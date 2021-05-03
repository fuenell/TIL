# 연관 컨테이너 (Associative Container)

연관 컨테이너는 `key - value` 구조를 가지는 컨테이너를 말한다 

## Set

`set`은 `key`에 대응하는 `value`가 없는 컨테이너다

특정값의 존재 여부를 확인할 때 자주 사용한다

특징으로는 값을 삽입하면 **자동으로 정렬되고, 중복값을 허용하지 않는다**

또한 **레드-블랙 트리** 형식으로 값이 저장되기 때문에 값 검색이 빠르다

- 임의의 값 원소 추가 및 제거 (`insert, erase`) : `O(log n)`
- 임의의 값 검색 (`find`) : `O(log n)`

```c++
#include <set>

int main() {
	set<int> s = { 40 };
	s.insert(30);
	s.insert(10);
	s.insert(20);

	s.erase(10);

	for (set<int>::iterator itr = s.begin(); itr != s.end(); ++itr)
		cout << *itr << " ";		// 정렬되어 출력된다

	if (s.find(20) != s.end())		// 검색 실패 시 end() 반복자를 반환한다
		cout << "Have[20]" << endl;
}
```

#### Multiset

중복을 허용하는 `set`이다

```c++
#include <set>

int main() {
	multiset<int> s = { 10 };
	s.insert(10);
	s.insert(10);
	s.insert(20);
	s.insert(20);

	s.erase(10);	// 해당 값 모두 삭제
}
```

## Map

`set` 구조에서 키에 대응하는 값도 추가된 컨테이너다

즉 **레드-블랙 트리 구조, 중복거부, 자동정렬**의 특징을 가진다

특이하게 `set[key]`로 원소에 접근했을 때 해당 키가 존재하지 않는다면 자동으로 원소를 추가해준다

때문에 `set[key] = 10`으로 수정 뿐안 아니라 삽입도 가능하다

- 임의의 값 원소 추가 및 제거 (`insert, erase`) : `O(log n)`
- 임의의 값 검색 (`find`) : `O(log n)`

```c++
#include <map>

int main() {
	map<string, int> m = { make_pair("십", 10) };

	// 아래 모든 삽입은 정상 작동한다
	m.insert(make_pair("이십", 20));
	m.insert(pair<string, int>("삼십", 30));
	m.insert(pair("사십", 40));
	m["오십"] = 50;

	m.erase("삼십");

	for (map<string, int>::iterator itr = m.begin(); itr != m.end(); ++itr)
		cout << itr->first << "/" << itr->second << " ";	// key 기준으로 정렬되어 출력된다

	if (m.find("이십") != m.end())		// 검색 실패 시 end() 반복자를 반환한다
		cout << "Have[20]" << endl;
}
```

#### Multimap

중복값을 허용하는 `map`이다

```c++
#include <map>

int main() {
	map<string, int> m = { make_pair("십", 10) };

	m.insert(make_pair("이십", 20));
	m.insert(make_pair("이십", 20));
	// m["오십"] = 50;	중복이 허용되므로 사용할 수 없음
}
```

## Unordered

해쉬 함수를 사용하기 때문에 순서가 없지만 속도가 아주 빠른 컨테이너다

- `insert, erase, find` : 평균 `O(1)`

#### Unordered_set

해쉬 함수를 사용하는 `set`이다

```c++
#include <unordered_set>

int main() {
	unordered_set<string> m = { "가", "나", "다" };

	m.insert("라");
	m.insert("마");

	for (const auto& item : m) {
		cout << item << endl;
	}

	if (m.find("가") != m.end())
		cout << "Have[가]";
}
```

#### Unordered_map

해쉬 함수를 사용하는 `map`이다

```c++
#include <unordered_map>

int main() {
	unordered_map<string, int> m = { make_pair("일", 1) };

	m["이"] = 2;
	m["삼"] = 3;

	for (const auto& item : m) {
		cout << item.first << ": " << item.second << endl;
	}

	if (m.find("이") != m.end())
		cout << "Have[2]";
}
```