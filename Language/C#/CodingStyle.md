# C#(Unity) 코딩 규칙

혼자서 코딩을 할 때에도 조금씩 코딩 규칙이 변형되는 것을 느껴,  
2023 신년맞이 내가 사용할 코딩 규칙을 확실하게 정하고 가기 위해 작성하는 문서이다.  
아래 Unity에서 제공하는 가이드라인을 많이 참고했다.  
https://resources.unity.com/games/create-code-style-guide-e-book

코딩 규칙에 정답은 없다.  
하지만 일관성은 중요하기 때문에 어떠한 규칙을 따르기로 했다면 해당 규칙을 계속 따라야 한다.

![image](https://user-images.githubusercontent.com/37904040/210523731-5d21f86e-890b-44f2-87ea-b5df1c0519ec.png)

## 중괄호
중괄호 스타일은 클래스, 함수, if, for 등 모든 경우에서 아래와 같이 작성한다.
### 🟢 Good
``` C#
for (int i = 0; i < length; i++)
{
    ...
}
```

### 🔵 Soso
``` C#
// 짧을 경우 아래 규칙 허용
if (a == 0) { ... }
```

### 🔴 Bad
``` C#
if (a == 0) {
    ...
}
```
``` C#
// 중괄호 생략 금지
for (int i = 0; i < length; i++)
    ...

```

## 접근 제한자
C#의 경우에는 멤버 변수의 기본 접근 제한자는 private이지만, (클래스의 default는 internal)  
의미적 구분을 위해서 모든 클래스와 변수에 접근 제한자를 붙여준다.
### 🟢 Good
``` C#
public class GameManager
{
    private int a;
    public int b;
}
```

### 🔵 Soso
``` C#
public class GameManager
{
    // 멤버 변수의 기본 접근 제한자가 private 이므로 private 생략 가능
    int a;
}
```

## 클래스명
클래스명은 Pascal 표기법을 사용한다.
### 🟢 Good
``` C#
public class GameManager { }
```

## 가상클래스
`Abs`를 접두사로 사용한 Pascal 표기법을 사용한다.
### 🟢 Good
``` C#
public abstract class AbsManager { }
```

## 인터페이스
`I`를 접두사로 사용한 Pascal 표기법을 사용한다.
### 🟢 Good
``` C#
public interface IMovable { }
```

## 함수명
함수명은 Pascal 표기법을 사용한다.
### 🟢 Good
``` C#
public void DoSomething() { }
```

## 변수
- const 멤버는 Upper(Screaming) Snake 표기법을 사용한다.  
- public 멤버는 Pascal 표기법을 사용한다.  
- private 멤버는 `_`를 접두사로 사용한 Camel 표기법을 사용한다.  
- 그외 매개변수, 지역변수는 Camel 표기법을 사용한다.
``` C#
public const string ABSOULTE_PATH = "...";
public static int Id;
public int Age;
private string _name;

public void Func(int parm)
{
    int local;
}
```

### 프로퍼티
프로퍼티는 (get, set) Pascal 표기법을 사용한다.
``` C#
public Name { get; set; }
```

### 어트리뷰트
어트리뷰트는 한 줄씩 윗줄에 작성한다.
``` C#
[SerializeField]
[ToolTip("설명")]
private string _text;

[SerializeField]
private GameObject _prefab;

// 어트리뷰트가 하나일 경우 왼쪽에 작성하는 것도 허용
[SerializeField] private GameObject _prefab;
```

## enum
enum명과 enum의 요소명 둘 다 Pascal 표기법을 사용한다.
``` C#
public enum FlowType
{
    Null,
    Play,
    Stop
}
```

## 변수 초기화
``` C#
// 일반적인 변수는 선언문에서 초기화
private int _initIndex = 0;
private List<int> _indexList = new List<int>();

private Collider _myCollider;
private Collider _enemyCollider;

// Awake에서는 자신의 컴포넌트를 초기화 
private void Awake()
{
    _myCollider = GetComponent<Collider>();
}

// Start에서는 다른 오브젝트의 컴포넌트를 초기화 
private void Start()
{
    _enemyCollider = Enemy.GetComponent<Collider>();
}
```

## bool 변수명, 함수명
``` C#
// is + (명사 or 현재진행형(~ing) or 형용사)
// has + (명사 or 과거분사)
// can + 동사원형

public bool IsSuccess()
{
    bool isFinished = false;
    ...
    return isFinished;
}
```


## 주석 스타일
코드 블럭, 구조, 프로세스 단계를 설명할 때에는 윗 주석 사용
부가 설명은 오른쪽 주석 사용
``` C#
// 이런 식으로 여러줄을 설명할 때는 윗 주석 작성
private GameObject GetPlayer()
{
    // 플레이어 생성
    GameObject player = Instantiate(prefab);

    // 플레이어 초기화
    player.name = "Player"; // 해당 줄의 부가 설명을 할 때는 오른쪽에 공백 한 칸을 두고 주석 작성
    player.SetActive(false);
    
    // 초기화된 플레이어를 반환
    return player;  
}
```
