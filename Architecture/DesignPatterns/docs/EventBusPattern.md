# 이벤트 버스 패턴 (Event Bus Pattern)
싱글톤 + 옵저버 패턴에서 enum으로 여러 종류의 이벤트를 나눔

``` C#
public class RaceEventBus
{
    public enum EventType
    {
        START, STOP, RESTART, QUIT
    }

    private static readonly IDictionary<EventType, UnityEvent> Events = new Dictionary<EventType, UnityEvent>();

    // EventType에 해당하는 이벤트 발생 시 호출될 함수 등록
    public static void Subscribe(EventType eventType, UnityAction listener)
    {
        UnityEvent thisEvent;

        if (Events.TryGetValue(eventType, out thisEvent))
        {
            thisEvent.AddListener(listener);
        }
        else
        {
            thisEvent = new UnityEvent();
            thisEvent.AddListener(listener);
            Events.Add(eventType, thisEvent);
        }
    }

    // 등록된 함수 해제
    public static void Unsubscribe(EventType type, UnityAction listener)
    {
        UnityEvent thisEvent;

        if (Events.TryGetValue(type, out thisEvent))
        {
            thisEvent.RemoveListener(listener);
        }
    }

    // EventType에 해당하는 이벤트 발생
    public static void Publish(EventType type)
    {
        UnityEvent thisEvent;

        if (Events.TryGetValue(type, out thisEvent))
        {
            thisEvent.Invoke();
        }
    }
}
```