# 데코레이터 패턴 (Decorato Pattern)
데코레이터 패턴은 객체에서 추가적인 요건을 동적으로 첨가한다  
서브클래스를 만드는 것을 통해 기능을 유연하게 확장할 수 있는 방법을 제공한다  
- `OCP(Open-Closed principle) 클래스 확장에 대해서는 열려 있지만 코드 변경에는 닫혀있는 구조`

## 다이어그램
새로운 음료, 토핑이 추가되면 그냥 새로운 클래스를 추가하면 된다
- `기존 코드를 변경하지 않으므로 버그 발생률을 낮출 수 있다`
- `클래스 개수가 많아진다`

![다이어그램-데코레이터](https://user-images.githubusercontent.com/37904040/107113846-f63c3c00-68a4-11eb-8b53-02801fe22579.png)

## 예제
더블 휘핑 핫초코 주문 시
``` C#

Beverage order1 = new HotChocolate();
order1 = new Whip(order1);
order1 = new Whip(order1);
order1.GetCost();   // 더블 휘핑 핫초코의 가격은 3달러
```
