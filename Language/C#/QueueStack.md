# 큐/스택 (Queue, Stack)

큐와 스택은 입력과 출력의 순서가 정해진 선형 자료구조이다

## Queue

`Queue`는 먼저 삽입된 데이터가 먼저 삭제되는 `선입선출(FIFO)`의 구조를 가지는 자료구조다

- 삽입, 삭제 (`Enqueue, Dequeue `) : `O(1)`

``` c#
Queue<int> q = new Queue<int>();
q.Enqueue(1);
q.Enqueue(2);
q.Enqueue(3);

Console.WriteLine(q.Peek()); // 첫 값 확인 

while (0 < q.Count)
    Console.WriteLine(q.Dequeue());
```

## Stack

`Stack`는 마지막에 삽입된 데이터가 먼저 삭제되는 `후입선출(LIFO)`의 구조를 가지는 자료구조다

- 삽입, 삭제 (`Push, Pop `) : `O(1)`

``` c#
Stack<int> s = new Stack<int>();
s.Push(1);
s.Push(2);
s.Push(3);

Console.WriteLine(s.Peek()); // 마지막 값 확인 

while (0 < s.Count)
    Console.WriteLine(s.Pop());
```