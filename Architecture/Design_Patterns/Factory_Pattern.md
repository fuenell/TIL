# 팩토리 패턴 (Factory Pattern)
객체를 생성하는 부분을 캡슐화하는 패턴

## 간단한 팩토리
디자인 패턴은 아니지만 객생 생성을 캡슐화하기 때문에 간단히 팩토리 패턴이 필요할 떄 사용할 수 있다  
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

