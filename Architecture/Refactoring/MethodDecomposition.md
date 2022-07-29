# 메소드 분해
리팩토링은 조금씩 조금씩 수정-테스트-수정-테스트의 반복으로 출력은 동일하나 코드를 유지보수하기 편하게 수정하는 과정이다.  
간단한 예제 메소드를 분해하면서 리팩토링의 과정을 이해한다.

영화 대여점 관리 프로그램 중 계산서를 출력하는 메소드다.
``` C#
public void PrintBill()
{
    float totalAmout = 0f;
    int point = 0;

    foreach (Rent rent in rentList)
    {
        switch (rent.GetMovie().GetPriceCode())
        {
            case Movie.NEW:
                totalAmout += 2000 * rent.GetRentDays();
                point += 3;
                break;
            case Movie.REGULAR:
                totalAmout += 1000 * rent.GetRentDays();
                point += 2;
                break;
            case Movie.OLD:
                totalAmout += 500 * rent.GetRentDays();
                point += 1;
                break;
        }
    }

    Console.WriteLine("총 대여료: " + totalAmout + "원");
    Console.WriteLine(point + "포인트 적립");
}
```

지금은 간단해서 메소드를 크게 수정할 필요가 없어 보인다.  
하지만 해당 계산서를 HTML에서 출력한다면 이 메소드를 통채로 복사해서 사용해야 할 것이다.  
그렇게 되면 대여료나 포인트를 계산하는 방식에서 변경사항이 생기면 코드의 일관성을 유지하기 위해 2개의 메소드를 각각 수정해줘야 할 것이다.  
따라서 해당 코드가 리팩토링이 필요하는 의미이다.  

먼저 totalAmout를 구하는 코드를 함수로 묶어서 꺼낸다.
``` C#
public void PrintBill()
{
    float totalAmout = GetTotalAmout(rentList);
    int point = 0;

    foreach (Rent rent in rentList)
    {
        switch (rent.GetMovie().GetPriceCode())
        {
            case Movie.NEW:
                point += 3;
                break;
            case Movie.REGULAR:
                point += 2;
                break;
            case Movie.OLD:
                point += 1;
                break;
        }
    }

    Console.WriteLine("총 대여료: " + totalAmout + "원");
    Console.WriteLine(point + "포인트 적립");
}

public float GetTotalAmout(List<Rent> rentList)
{
    foreach (Rent rent in rentList)
    {
        switch (rent.GetMovie().GetPriceCode())
        {
            case Movie.NEW:
                totalAmout += 2000 * rent.GetRentDays();
                break;
            case Movie.REGULAR:
                totalAmout += 1000 * rent.GetRentDays();
                break;
            case Movie.OLD:
                totalAmout += 500 * rent.GetRentDays();
                break;
        }
    }
}
```
이후 테스트 후 이번에는 포인트를 계산하는 코드를 함수로 묶어낸다.
``` C#
public void PrintBill()
{
    float totalAmout = GetTotalAmout(rentList);
    int point = GetTotalPoint(rentList);

    Console.WriteLine("총 대여료: " + totalAmout + "원");
    Console.WriteLine(point + "포인트 적립");
}

public float GetTotalPoint(List<Rent> rentList)
{
    foreach (Rent rent in rentList)
    {
        switch (rent.GetMovie().GetPriceCode())
        {
            case Movie.NEW:
                point += 3;
                break;
            case Movie.REGULAR:
                point += 2;
                break;
            case Movie.OLD:
                point += 1;
                break;
        }
    }
}
```
