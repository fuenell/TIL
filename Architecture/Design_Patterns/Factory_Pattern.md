# 팩토리 패턴 (Factory Pattern)
객체를 생성하는 부분을 캡슐화하는 패턴  
코드에서 `new` 키워드를 사용한다면 해당 코드는 구상 객체를 의존하고 있다는 의미다  
그렇게 된다면 해당 코드는 유연성이 떨어지고 수정해야할 가능성이 증가한다  
그렇기에 팩토리 패턴을 사용해 객체 생성을 캡슐화 해 (변경되는 부분을 캡슐화)  
해당 부분에서 변경사항이 발생하더라도 클라이언트에서 코드를 수정할 필요가 없는 코드를 만들 수 있다

## 간단한 팩토리 (Simple Factory)
세부적으로 말하자면 간단한 팩토리는 디자인 패턴은 아니지만 (관용구에 가까움)  
단순하게 객체를 생성하는 부분을 캡슐화할 수 있다  
``` C#
public SpwanMonster(string type)
{
    Monster monster = MonsterFactory.CreateMonster(type);
    monster?.SetState(0);
}
```
``` C#
public class MonsterFactory
{
    public static Monster CreateMonster(string type)
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

## 팩토리 메소드 패턴
``` C#
using System;

public class Class2
{
    IMonsterFactory m_MonsterFactory;

    Class2(IMonsterFactory monsterFactory)
    {
        m_MonsterFactory = monsterFactory;
    }

    public SpwanMonster(string size)
    {
        Monster monster = m_MonsterFactory.CreateMonster(size); ;

        monster?.SetState(0);
    }
}

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

