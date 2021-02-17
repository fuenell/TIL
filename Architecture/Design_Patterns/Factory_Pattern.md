# 팩토리 패턴 (Factory Pattern)
객체를 생성하는 부분을 캡슐화하는 패턴  
코드에서 `new` 키워드를 사용한다면 해당 코드는 구상 객체를 의존하고 있다는 의미다  
그렇게 된다면 해당 코드는 유연성이 떨어지고 수정해야할 가능성이 증가한다  
그렇기에 팩토리 패턴을 사용해 객체 생성을 캡슐화 해 (변경되는 부분을 캡슐화)  
객체 생성부분에서 변경사항이 발생할 때, 팩토리 패턴 부분만 수정하면 되는 코드를 만들 수 있다 

## 간단한 팩토리 (Simple Factory)
세부적으로 말하자면 간단한 팩토리는 디자인 패턴은 아니지만 (관용구에 가까움)  
단순하게 객체를 생성하는 부분을 캡슐화할 수 있다  
``` C#
class Program
{
    static void Main(string[] args)
    {
        new MonsterController(new MonsterFactory()).SpwanMonster("spider");
    }
}
```
``` C#
class MonsterController
{
    MonsterFactory m_MonsterFactory;

    public MonsterController(MonsterFactory monsterFactory)
    {
        m_MonsterFactory = monsterFactory;
    }

    public void SpwanMonster(string type)
    {
        Monster monster = m_MonsterFactory.CreateMonster(type);
        monster?.SetState(0);
    }
}
```
``` C#
public class MonsterFactory
{
    public Monster CreateMonster(string type)
    {
        Monster monster;

        switch (type)
        {
            case "zombie":
                monster = new Zombie();
                break;

            case "spider":
                monster = new Spider();
                break;

            default:
                monster = null;
                break;
        }

        return monster;
    }
}
```
예를 들어 게임의 난이도가 상승해 생성되는 몬스터들은 빨갛고 변하고 더욱 강해져야 한다면  
`MonsterFactory`의 서브 클래스로 `RedMonsterFactory`를 구현해 (이때 `MonsterFactory.CreateMonster()`을 `virtual`로 두거나 추상 메소드로 변경해주어야 함수 재정의가 가능하다)  
객체를 초기화 하는 부분에서 해당 팩토리를 대신 넣는다면 위 코드는 전혀 수정하지 않고 변경 사항을 적용할 수 있다
``` C#
class Program
{
    static void Main(string[] args)
    {
        new MonsterController(new RedMonsterFactory()).SpwanMonster("spider");
    }
}
```
``` C#
public class RedMonsterFactory : MonsterFactory
{
    public override Monster CreateMonster(string type)
    {
        Monster monster;

        switch (type)
        {
            case "zombie":
                monster = new RedZombie();
                break;

            case "spider":
                monster = new RedSpider();
                break;

            default:
                monster = null;
                break;
        }

        return monster;
    }
}
```

## 정적 팩토리 (Static Factory)
정적 팩토리는 간단한 팩토리의 연장선이다  
팩토리 메소드를 정적으로 선언해 팩토리 객체의 인스턴스를 생성하지 않아도 된다는 장점이 있다  
하지만 서브클래스를 만들어 메소드의 행동을 변경할 수 없다는 단점도 존재한다  
``` C#
class MonsterController
{
    public void SpwanMonster(string type)
    {
        Monster monster = MonsterFactory.CreateMonster(type);
        monster?.SetState(0);
    }
}
```
``` C#
class MonsterFactory
{
    public static Monster CreateMonster(string type)
    {
        // ...
    }
}
```

## 팩토리 메소드 패턴
팩토리 메소드 패턴은 객체를 생성하는 메소드를 추상 메소드로 두어  
객체의 생성을 각 서브 클래스에 맡긴다
``` C#
class MonsterSpawner
{
    public void SpawnMonster(string size)
    {
        Monster monster = CreateMonster(size); ;

        monster?.SetState(0);
    }

    public class MonsterFactory
    {
        public abstract Monster CreateMonster(string type)
        {
            /// ...
        }
    }
}
```
``` C#
class RedMonsterSpawner : MonsterSpawner
{
    public class MonsterFactory
    {
        public abstract Monster CreateMonster(string type)
        {
            /// ...
        }
    }
}
```

## 추상 팩토리 패턴
public interface IMonsterFactory
{
    Monster CreateMonster(string type);
}

public class ZomieFactory : IMonsterFactory
{
    public static Monster CreateMonster(string size)
    {
        Zombie zombie;

        switch (size)
        {
            case "small"
                zombie = new SmallZombie();
                break;

            case "big"
                zombie = new BigZombie();
                break;

            default:
                zombie = null;
                break;
        }

        return zombie;
    }
}

public class SpiderFactory : IMonsterFactory
{
    public static Monster CreateMonster(string size)
    {
        Spider spider;

        switch (size)
        {
            case "small"
                spider = new SmallSpider();
                break;

            case "big"
                spider = new BigSpider();
                break;

            default:
                spider = null;
                break;
        }

        return spider;
    }
}

public abstract class Monster
{
    public void SetState(int state) { }
}

public abstract class Zombie : Monster { }
public class SmallZombie : Zombie { }
public class BigZombie : Zombie { }

public abstract class Spider : Monster { }
public class SmallSpider : Spider { }
public class BigSpider : Spider { }
```

## 추상 팩토리 패턴

