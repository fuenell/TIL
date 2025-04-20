# 퍼사드 패턴 (Facade Pattern)
Facade 은 건물 정면의 외벽을 뜻하는 말이다.

퍼사드 패턴은 복잡한 시스템을 단순화해서 외부에서 쉽게 조정할 수 있게 묶는 패턴이다.

외부 코드가 이 퍼사드만 사용하면 내부의 복잡한 구성요소들을 신경 쓰지 않아도 된다.

``` C#
public class FacadeTest : MonoBehaviour
{
    private Computer myComputer;

    void Start()
    {
        myComputer = new Computer();
        myComputer.TurnOn();    // 구체적으로 어떤 동작을 하는지 몰라도 상관없다
    }
}

public class Computer
{
    private PowerSupply powerSupply;
    private BootLoader bootLoader;
    private OperatingSystem operatingSystem;

    public Computer()
    {
        powerSupply = new PowerSupply();
        bootLoader = new BootLoader();
        operatingSystem = new OperatingSystem();
    }

    public void TurnOn()
    {
        powerSupply.PowerOn();
        bootLoader.Boot();
        operatingSystem.Load();
    }

    public void TurnOff()
    {
        operatingSystem.Shutdown();
        powerSupply.PowerOff();
    }
}
```