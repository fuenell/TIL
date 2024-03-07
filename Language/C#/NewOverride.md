# 가상 함수 상속 (new, override)

# 요약
#### new = 같은 이름의 새로운 함수를 선언
#### override = 기존 함수를 재정의한다


### virtual
먼저 override를 사용하려면 함수에 virtual 키워드를 붙여 가상 함수로 선언해야 합니다.
``` C#
class Parent
{
    virtual void Test()
    {
    
    }
}
```

### new
