# VContainer
VContainer는 Unity에서 의존성 주입을 편하게 할 수 있게 해주는 프레임워크다.

## 의존성 주입이란?
필요한 객체를 스스로 만드는 것이 아니라 외부에서 주입해 주는 것을 말한다.

``` C#
public class Player {
    private Weapon weapon;

    public Player() {
        weapon = new Sword(); // 직접 생성 → 결합도 높음
    }
}

public class Player {
    private Weapon weapon;

    public Player(Weapon weapon) {
        this.weapon = weapon; // 외부에서 주입
    }
}
```

### 의존성 주입을 사용하는 이유
- 코드 결합도를 낮추기 위해
- 테스트를 쉽게 하기 위해


## 설치 방법
UPM에서 URL로 설치
```
https://github.com/hadashiA/VContainer.git?path=VContainer/Assets/VContainer
```


## 사용 방법
LifetimeScope 를 상속받은 클래스를 선언 후
Configure 함수를 재정의해서 사용할 클래스들을 모두 등록해 주고 진입점을 정해준다.

``` C#
public class GameLifetimeScope : LifetimeScope {
    // 등록 순서는 중복 등록만 아니면 상관 없다 (중복 등록의 경우 마지막 등록만 유효)
    protected override void Configure(IContainerBuilder builder) {
        builder.Register<IWeapon, Sword>(Lifetime.Singleton);   // Sword를 싱글톤으로 등록해 IWeapon 타입이 필요하면 Sword를 반환한다.
        builder.RegisterEntryPoint<Game>();     // 진입점 등록
    }
}

public class Game : IStartable {
    private readonly IWeapon weapon;

    // builder.RegisterEntryPoint<Game>() 를 호출하면 자동으로 생성자가 불리면서 객체 생성
    // IWeapon가 빌더에 등록되어 있기 때문에 성공적으로 Sword를 의존성 주입
    // 만약 IWeapon가 빌더에 등록되어 있지 않다면 예외 발생
    public Game(IWeapon weapon) {
        this.weapon = weapon;
    }

    // builder.RegisterEntryPoint로 Start 함수가 호출됨
    public void Start() {
        weapon.Attack();
    }
}
```

## Register 타입
객체를 등록할 때 아래 3가지 타입 중 하나를 선택할 수 있다.
| 생명주기        | 언제 생성됨?    | 예시              |
| ----------- | ---------- | --------------- |
| `Singleton` | 한 번만       | 게임 매니저          |
| `Scoped`    | Scope마다 1회 | UI 컨트롤러, 화면별 로직 |
| `Transient` | 요청마다       | 데이터 DTO, 임시 유틸  |


## Scope
Scope는 VContainer의 핵심적인 개념 중 하나이다.
``` C#
var battleScope = lifetimeScope.CreateChild(BattleLifetimeScopePrefab);
```