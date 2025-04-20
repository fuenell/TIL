# 방문자 패턴 (Visitor Pattern)
방문하는 클래스마다 다른 동작을 처리하는 패턴

핵심은 "행동(방문자)을 객체에서 분리하고, 객체는 방문만 허용한다"는 구조다.

객체는 자신이 어떤행동을 하는지 모르지만
방문자는 방문할 객체를 모두 알아야 하기 때문에 

``` C#
// 방문자 인터페이스: 각 타입별 방문 메서드를 정의
public interface IVisitor
{
    void Visit(Player player);
    void Visit(Enemy enemy);
}

// 방문자의 구체 구현: "Health Boost"라는 행위를 각 타입에 맞게 구현
public class HealthBoostVisitor : IVisitor
{
    public void Visit(Player player)
    {
        // Player는 20 회복
        player.health += 20;
    }

    public void Visit(Enemy enemy)
    {
        // Enemy는 10 회복
        enemy.health += 10;
    }
}
```
``` C#
// 방문 가능한 요소 인터페이스: IVisitor를 받아들이는 메서드 정의
public interface IElement
{
    void Accept(IVisitor visitor);
}

// Player 클래스는 방문을 허용하는 대상 (Element)
public class Player : MonoBehaviour, IElement
{
    public float health = 100;

    // 방문자를 받아서, 나(Player)를 방문하게 위임함
    public void Accept(IVisitor visitor)
    {
        visitor.Visit(this);  // this는 현재 Player 인스턴스
    }
}

// Enemy 클래스도 마찬가지로 방문 허용
public class Enemy : MonoBehaviour, IElement
{
    public float health = 50;

    public void Accept(IVisitor visitor)
    {
        visitor.Visit(this);  // this는 현재 Enemy 인스턴스
    }
}
```
``` C#
// 방문자 패턴을 사용하는 클라이언트 역할
public class GameManager : MonoBehaviour
{
    public Player player;
    public Enemy enemy;

    // 공통된 방문자(행동): Health Boost
    private IVisitor healthBoost = new HealthBoostVisitor();

    private void Start()
    {
        // 각 객체에게 방문자를 전달함
        player.Accept(healthBoost); // Player → HealthBoostVisitor.Visit(Player)
        enemy.Accept(healthBoost);  // Enemy → HealthBoostVisitor.Visit(Enemy)

        // 이렇게 객체는 '방문을 허용'만 하고
        // 실제 행동은 Visitor가 책임짐
    }
}
```