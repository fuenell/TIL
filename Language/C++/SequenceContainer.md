# 시퀀스 컨테이너 (Sequence Container)

STL 컨테이너는 기본적인 자료구조들이기 때문에 자주 사용하므로, C++에서는 필수로 배워야 한다

크게 시퀀스 컨테이너인 `vector, list, deque`와 연관 컨테이너인 `set, map`으로 나눌 수 있다

먼저 시퀀스 컨테이너에 대하여 알아보겠다

## Iterator

`iterator`는 아래의 컨테이너들을 비슷한 방식으로 원소에 접근하기 위한 반복자이다

`vector`의 경우 `vector.begin()`과 `vector.end()`로 첫 원소의 위치와 마지막 원소 + 1위치를 얻을 수 있다

```c++
for (vector<int>::iterator itr = vec.begin(); itr != vec.end(); itr++) {
	cout << *itr << endl;
}

// 다음과 같이 간단하게 사용할 수도 있다
for (const auto& elem : m) {
    cout << elem << endl;
}
```

## Vector

`vector`는 가변길이 배열이다

실제로 메모리에도 순차적으로 저장되고 임의 원소를 접근하는 것이 매우 빠르다

정해진 크기를 초과하면 더 큰 메모리를 할당하고 원소를 복사하며 `O(n)`비용이 발생하는 단점이 있다

- 임의의 위치 원소 접근 (`[], at`) : `O(1)`
- 맨 뒤에 원소 추가 및 제거 (`push_back, pop_back`) : `O(1)`, (크기 확장 시 `O(n)`)
- 임의의 위치 원소 추가 및 제거 (`insert, erase`) : `O(n)` (원소들을 한칸씩 밀어야하기 때문에)

```C++
#include <vector>

int main() {
    vector<int> vec = { 1, 2, 3 };

    vec.pop_back();     // 맨 뒤에 3 제거    {1, 2}
    vec.push_back(5);   // 맨 뒤에 5 추가    {1, 2, 5}

    vec.insert(vec.begin() + 1, 10);  // 2번째 위치에 10 추가      {1, 10, 2, 5}
    vec.erase(vec.end() - 1);         // 마지막 위치 원소 5 제거    {1, 10, 2}

    for (vector<int>::size_type i = 0; i < vec.size(); i++) {
        cout << vec[i] << endl;
    }
}
```

## List

`list`는 노드로 연결된 양방향 연결리스트다

때문에 임의 원소 접근이 불가능하고, 리스트의 반복자는 `++, --`연산만 가능하다

- 맨 뒤에 원소 추가 및 제거 (`push_back, pop_back`) : `O(1)`
- 임의의 위치 원소 추가 및 제거 (`insert, erase`) : `O(1)` (반복자를 한칸씩 움직여야 해서 사실상 `O(n)`)

```C++
#include <list>

int main() {
	list<int> lst = { 1, 2, 3 };

	lst.pop_back();     // 맨 뒤에 3 제거
	lst.push_back(5);  // 맨 뒤에 4 추가

	lst.insert(++lst.begin(), 10);  // 2번째 위치에 10 추가
	lst.erase(--lst.end());  // 마지막 위치 원소 5 제거

	for (list<int>::iterator itr = lst.begin(); itr != lst.end(); ++itr) {
		cout << *itr << endl;
	}
}
```

## Deque

`deque`는 `vector`에다가 `O(1)`비용으로 맨 앞에 원소를 추가하는 함수가 추가된 배열이다

또한 정해진 크기를 초과하면 일정량 메모리를 추가 할당해 배열을 이어 붙인다

때문에 `deque`는 `vector`보다 메모리를 많이 사용하지만 속도는 더 빠르다

- 임의의 위치 원소 접근 (`[], at`) : `O(1)`

- 맨 앞에 원소 추가 및 제거 (`push_front, pop_front`) : `O(1)`

- 맨 뒤에 원소 추가 및 제거 (`push_back, pop_back`) : `O(1)`

- 임의의 위치 원소 추가 및 제거 (`insert, erase`) : `O(n)` (원소들을 한칸씩 밀어야하기 때문에)

```C++
#include <deque>

int main() {
    deque<int> dq = { 1, 2, 3 };

    dq.pop_front();     // 맨 앞에 1 제거    {2, 3}
    dq.push_front(0);   // 맨 앞에 0 추가    {0, 2, 3}

    dq.pop_back();     // 맨 뒤에 3 제거     {0, 2}
    dq.push_back(5);   // 맨 뒤에 5 추가     {0, 2, 5}

    dq.insert(dq.begin() + 1, 10);  // 2번째 위치에 10 추가     {0, 10, 2, 5}
    dq.erase(dq.end() - 1);         // 마지막 위치 원소 5 제거   {0, 10, 2}

    for (deque<int>::size_type i = 0; i < dq.size(); i++) {
        cout << dq[i] << endl;
    }
}
```
