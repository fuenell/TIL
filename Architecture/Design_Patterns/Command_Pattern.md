# 커맨드 패턴 (Command Pattern)
커맨드 패턴은 특정 요청을 캡슐화해 요청하는 객체와 그 요청을 수행하는 객체를 분리시키는 패턴이다  
쉽게 말하자면 한 차원 높은 수준의 옵버저 패턴이다  

``` C#
class Program
{
    static void Main(string[] args)
    {
        TV tv = new TV();
        ICommand tvOnCommand = new TVOnCommand(tv);
        ICommand tvOffCommand = new TVOffCommand(tv);

        Fan fan = new Fan();
        ICommand fanOnCommand = new FanOnCommand(fan);
        ICommand fanOffCommand = new FanOffCommand(fan);

        ICommand allOffCommand = new MacroCommand(new ICommand[] { tvOffCommand, fanOffCommand });

        SmartRemoteController src = new SmartRemoteController();
        src.SetCommand(0, tvOnCommand);
        src.SetCommand(1, tvOffCommand);
        src.SetCommand(2, fanOnCommand);
        src.SetCommand(3, allOffCommand);

        src.ClickButton(0);
        src.ClickButton(1);
        src.ClickButton(2);
        src.ClickButton(3);
        src.ClickUndo();
    }
}
```
``` C#
public interface ICommand
{
    public void Execute();
    public void Undo();
}

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
    }

    public void SetCommand(int index, ICommand command)
    {
        if (0 <= index && index < commands.Length)
            commands[index] = command;
    }

    public void ClickButton(int index)
    {
        prevCommand = commands[index];
        commands[index].Execute();
    }

    public void ClickUndo()
    {
        prevCommand.Undo();
    }
}
```
