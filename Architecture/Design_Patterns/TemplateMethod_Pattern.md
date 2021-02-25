# 템플릿 메소드 패턴 (TemplateMethod Pattern)
템플릿 메소드 패턴은 알고리즘의 일부 단계를 서브클래에서 구현하는 패턴이다  
팩토리 메소드 패턴과 굉장히 유사하다  
어떻게 보면 팩토리 메소드 패턴은 템플릿 메소드 패턴을 적용한 팩토리 패턴으로 볼 수도 있다

## 예제
#### 커피를 만드는 방법
```
1. 물을 끓인다
2. 끓는 물에 커피를 우려낸다
3. 커피를 컵에 따른다
4. 설탕을 추가한다
```
#### 차를 만드는 방법
```
1. 물을 끓인다
2. 끓는 물에 차를 우려낸다
3. 차를 컵에 따른다
```
이 두가지 알고리즘을 템플릿 메소드 패턴으로 묶을 수 있다
``` C#
static void Main(string[] args)
{
    HotBeverage tea = new Tea();
    HotBeverage coffee = new Coffee();

    tea.PrepareBeverage();
    coffee.PrepareBeverage();
}
```
``` C#
public abstract class HotBeverage
{
    public void PrepareBeverage()
    {
        BoilWater();
        Brew();
        PourInCup();
        AddCondiments();
    }

    private void BoilWater()
    {
        Console.WriteLine("물을 끓인다");
    }

    private void PourInCup()
    {
        Console.WriteLine("만들어진 음료를 컵에 따른다");
    }

    public abstract void Brew();        // 서브 클래스에서 구현해야 하는 메소드

    public void AddCondiments() { }     // Hook 메소드 구현해도 되고 안해도 상관없는

}

public class Coffee : HotBeverage
{
    public override void Brew()
    {
        Console.WriteLine("물에 커피를 우려낸다");
    }
    public new void AddCondiments()
    {
        Console.WriteLine("설탕을 추가한다");
    }
} 

public class Tea : HotBeverage
{
    public override void Brew()
    {
        Console.WriteLine("물에 차를 우려낸다");
    }
}
```
