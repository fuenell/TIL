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
    // 등록 순서는 중복 등록만 아니면 상관 없다
    protected override void Configure(IContainerBuilder builder) {
        builder.Register<IWeapon, Sword>(Lifetime.Singleton);   // Sword를 싱글톤으로 등록해 IWeapon 타입이 필요하면 Sword를 반환한다.
        builder.RegisterEntryPoint<Game>();     // 진입점 등록
    }
}

public class Game : IStartable {
    private readonly IWeapon weapon;

    // builder.RegisterEntryPoint<Game>() 를 호출하면 자동으로 생성자가 불리면서 생성된다.
    // IWeapon가 빌더에 등록되어 있기 때문에 성공적으로 Sword를 의존성 주입한다.
    public Game(IWeapon weapon) {
        this.weapon = weapon;
    }
}
```
