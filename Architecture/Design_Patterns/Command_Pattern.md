# 커맨드 패턴 (Command Pattern)
커맨드 패턴은 특정 요청을 캡슐화해 요청하는 객체와 그 요청을 수행하는 객체를 분리시키는 패턴이다  
쉽게 말하자면 한 차원 높은 수준의 옵버저 패턴이다   

### 특징
- 각 커맨드는 원하는 함수를 호출할 수 있어, 리시버 클래스의 코드를 수정하거나 하나의 인터페이스로 묶을 필요가 없다
- 실행 취소, 로그 저장, 큐에 저장 후 하나씩 실행 등 각 커맨드를 여러 방식으로 다룰 수 있다
- 각 커맨드마다 클래스를 만들어야 하기 때문에 클래스가 너무 많아질 수 있다 

## 스마트 리모컨
4개의 버튼과 실행취소 버튼이 달려있는 스마트 리모컨이 있다  
각 버튼에는 원하는 기능을 작동시키는 명령어를 넣을 수 있다  
이런 상황일 때 각 명령어들을 캡슐화하는 커맨드 패턴을 적용시킬 수 있다  

아래 예제에서는 각 버튼은 다음과 같은 명령어를 저장한다
```
1번 - TV를 켠다  
2번 - TV를 끈다  
3번 - 선풍기를 켠다  
4번 - 모든 기기를 끈다  
실행취소 - 최근 실행한 명령어를 실행하기 전 상태로 되돌린다  
```

``` C#
class Program
{
    static void Main(string[] args)
    {
        TV tv = new TV();   // 커맨드를 최종적으로 수행하는 리시버
        ICommand tvOnCommand = new TVOnCommand(tv);     // TV를 켜는 커맨드
        ICommand tvOffCommand = new TVOffCommand(tv);   // TV를 끄는 커맨드

        Fan fan = new Fan();
        ICommand fanOnCommand = new FanOnCommand(fan);
        ICommand fanOffCommand = new FanOffCommand(fan);

        ICommand allOffCommand = new MacroCommand(new ICommand[] { tvOffCommand, fanOffCommand });     // TV와 Fan을 끄는 커맨드

        SmartRemoteController src = new SmartRemoteController();    // 커맨드를 보관, 실행 시키는 인보커
        src.SetCommand(0, tvOnCommand);     // 0번 버튼에 TV를 켜는 커맨드 저장
        src.SetCommand(1, tvOffCommand);
        src.SetCommand(2, fanOnCommand);
        src.SetCommand(3, allOffCommand);

        src.ClickButton(0);     // 0번에 저장된 커맨드 실행
        src.ClickButton(1);
        src.ClickButton(2);
        src.ClickButton(3);
        src.ClickUndo();        // 실행 취소 커맨드 실행
    }
}
```
``` C#
public interface ICommand
{
    public void Execute();
    public void Undo();
}

// null 객체 (null을 대체)
class NullCommand : ICommand
{
    public void Execute() { }
    public void Undo() { }
}

class TVOnCommand : ICommand
{
    private TV tv;

    public TVOnCommand(TV tv)
    {
        this.tv = tv;
    }

    public void Execute()
    {
        tv.On();
    }
    public void Undo()
    {
        tv.Off();
    }
}
class TVOffCommand : ICommand
{
    private TV tv;

    public TVOffCommand(TV tv)
    {
        this.tv = tv;
    }

    public void Execute()
    {
        tv.Off();
    }
    public void Undo()
    {
        tv.On();
    }
}

class FanOnCommand : ICommand
{
    private Fan fan;
    private int prevSpeed;

    public FanOnCommand(Fan fan)
    {
        this.fan = fan;
    }

    public void Execute()
    {
        prevSpeed = fan.GetSpeed();
        fan.SetSpeed(3);
    }

    public void Undo()
    {
        fan.SetSpeed(prevSpeed);
    }
}
class FanOffCommand : ICommand
{
    private Fan fan;
    private int prevSpeed;

    public FanOffCommand(Fan fan)
    {
        this.fan = fan;
    }

    public void Execute()
    {
        prevSpeed = fan.GetSpeed();
        fan.SetSpeed(0);
    }

    public void Undo()
    {
        fan.SetSpeed(prevSpeed);
    }
}

// 여러 커맨드를 한번에 실행 시키는 MacroCommand
class MacroCommand : ICommand
{
    public ICommand[] commands;

    public MacroCommand(ICommand[] commands)
    {
        this.commands = commands;
    }

    public void Execute()
    {
        for (int i = 0; i < commands.Length; i++)
            commands[i].Execute();
    }

    public void Undo()
    {
        for (int i = 0; i < commands.Length; i++)
            commands[i].Undo();
    }
}
```
``` C#
public class SmartRemoteController
{
    private ICommand[] commands;
    private ICommand prevCommand;

    public SmartRemoteController()
    {
        commands = new ICommand[4];
        for (int i = 0; i < commands.Length; i++)
            commands[i] = new NullCommand();
         prevCommand = new NullCommand();
    }

    // 각 버튼에 커맨드를 저장한다
    public void SetCommand(int index, ICommand command)
    {
        if (0 <= index && index < commands.Length)
            commands[index] = command;
    }

    // 버튼을 눌러 해당 커맨드를 실행시킨다
    public void ClickButton(int index)
    {
        if (0 <= index && index < commands.Length)
        {
            prevCommand = commands[index];
            commands[index].Execute();      // null 객체로 초기화했기 때문에 ==null if문을 걸 필요가 없다
        }
    }

    // 최근 실행한 커맨드의 Undo() 함수를 실행시킨다
    public void ClickUndo()
    {
        prevCommand.Undo();      // 이전 커맨드가 없더라도 NullCommand.Execute()를 실행하기 때문에 아무일도 일어나지 않는다
    }
}
```
