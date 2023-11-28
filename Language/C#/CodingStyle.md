# C#(Unity) 코딩 규칙

혼자서 코딩을 할 때에도 조금씩 코딩 규칙이 변형되는 것을 느껴,  
2023 신년맞이 내가 사용할 코딩 규칙을 확실하게 정하고 가기 위해 작성하는 문서이다.  
아래 Unity에서 제공하는 가이드라인을 많이 참고했다.  
https://resources.unity.com/games/create-code-style-guide-e-book

코딩 규칙에 정답은 없다.  
하지만 일관성은 중요하기 때문에 어떠한 규칙을 따르기로 했다면 해당 규칙을 계속 따라야 한다.

![image](https://user-images.githubusercontent.com/37904040/210523731-5d21f86e-890b-44f2-87ea-b5df1c0519ec.png)

## 명명규칙
### 클래스
Pascal 표기법을 사용한다.
``` C#
public class GameManager { ... }
```
### 함수
Pascal 표기법을 사용한다.
``` C#
public void DoSomething() { ... }
```
### 변수
+ public 멤버는 Pascal 표기법을 사용한다.
+ private 멤버는 `_`를 접두사로 사용한 Camel 표기법을 사용한다.
``` C#
private string _name;
public int Age;
```
const 멤버는 Upper(Screaming) Snake 표기법을 사용한다. 
``` C#
public const string ABSOULTE_PATH = "...";
```
그외 매개변수, 지역변수는 Camel 표기법을 사용한다.
``` C#
public void Func(int parm)
{
    int local;
}
```
### 인터페이스
`I`를 접두사로 사용한 Pascal 표기법을 사용한다.
``` C#
public interface IMovable { ... }
```
### enum
enum명과 enum의 요소명 둘 다 Pascal 표기법을 사용한다.
``` C#
public enum FlowType
{
    Null,
    Play,
    Stop
}
```
### 프로퍼티
Pascal 표기법을 사용한다.
``` C#
public Name { get; set; }
```

## 접근 제한자
의미적 구분을 위해서 모든 클래스와 변수에 접근 제한자를 붙여준다.
##### 🟢 Good
private은 생략 가능하다.
``` C#
public class GameManager
{
    private int _a;
    public int B;
}
```

## 중괄호
클래스, 함수, if, for 등 모든 경우에서 아래와 같이 작성한다.
##### 🟢 Good
``` C#
for (int i = 0; i < length; i++)
{
    ...
}
```
##### 🔵 Soso
짧을 경우, 아래 규칙도 허용한다.
``` C#
if (a == 0) { ... }
```
##### 🔴 Bad
``` C#
if (a == 0) {
    ...
}
```
중괄호는 생략하지 않는다.
``` C#
for (int i = 0; i < length; i++)
    ...
```
## 변수 초기화
+ 일반적인 변수는 선언문에서 초기화한다.
+ Awake에서는 자신의 컴포넌트를 초기화한다.
+ Start에서는 다른 오브젝트의 컴포넌트를 초기화한다.
``` C#
private int _initIndex = 0;
private List<int> _indexList = new List<int>();

private void Awake()
{
    _myCollider = GetComponent<Collider>();
}

private void Start()
{
    _enemyCollider = Enemy.GetComponent<Collider>();
}
```
