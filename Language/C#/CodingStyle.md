# C#(Unity) 코딩 규칙

혼자서 코딩을 할 때에도 조금씩 코팅 규칙이 변형되는 것을 느껴,  
2023 신년맞이 내가 사용할 코딩 규칙을 확실하게 정하고 가기 위해 작성하는 문서이다.  
아래 유티니에서 제공하는 가이드라인을 많이 참고했다.  
https://resources.unity.com/games/create-code-style-guide-e-book

## 중괄호
중괄호 스타일은 크게 K&R, BSD로 나뉘는데 VisualStudio(C#)에서 코드 정렬 시  
자동으로 BSD 방식이 적용되기 때문에 BSD 방식을 사용한다.
``` C#
public class A
{
    private void FuncA()
    {
        if (a == 0)
        {
            for (int i = 0; i < 10; i++)
            {
                // 모든 경우에 BSD 방식을 사용
            }
        }

        // 예외적으로 코드 블럭 사이의 내용이 한 줄일 경우 다음과 같이 작성하는 것도 허용
        if (a <= 2) { a = 2; }
        else if (a <= 4) { a = 4; }
        else if (a <= 8) { a = 8; }
    }
}
```
#### 금지 규칙
``` C#
// K&R 방식 사용 금지
public class A {
    private void SetA() {

        // for, if문에서 중괄호 생략 금지
        for (i = 0; i < 10; i++)
            if(i % 2 == 0)
                print(i);

    }
}
```

## 접근 제한자
C#의 경우에는 멤버 변수의 기본 접근 제한자는 private이지만, (클래스의 default는 internal)  
의미적 구분을 위해서 모든 클래스와 변수에 접근 제한자를 붙여준다.
``` C#
public class GameManager
{
    private int a;
    public int b;
}
```

## 클래스명
클래스명은 Pascal 표기법을 사용한다.
``` C#
public class GameManager { }
```

## 가상클래스
`Abs`를 접두사로 사용한 Pascal 표기법을 사용한다.
``` C#
public abstract class AbsManager { }
```

## 인터페이스
`I`를 접두사로 사용한 Pascal 표기법을 사용한다.
``` C#
public interface IMovable { }
```

## 함수명
함수명은 Pascal 표기법을 사용한다.
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
```

## enum
enum명과 enum의 요소명 둘 다 Pascal 표기법을 사용한다.
``` C#
public enum FlowType { Null, Play, Stop }
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

## 배열, 리스트 명명 규칙
``` C#
int[] apples; // 배열 변수 명은 복수형 (사전 참조, 셀 수 없는 단어는 코딩적 허용으로 s를 붙임)
List<int> appleList; // 리스트는 List를 접미사로 사용
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
    player.name = "Player"; // 해당 줄의 부가 설명을 할 때는, 오른쪽에 공백 한 칸을 두고 주석 작성
    player.SetActive(false);
    
    // 초기화된 플레이어를 반환
    return player;  
}
```
