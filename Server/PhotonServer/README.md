# 포톤 서버 (Photon Server)
보통 유니티로 멀티 게임을 만들 때 자주 사용되는 서버 솔루션

## 포톤 퓨전
유니티 특화 서버


## 포톤 퀀텀

|항목|Fusion|Quantum|
|-|-|-|
|목적|부드럽고 유연한 실시간 플레이|정확하고 공정한 경쟁 환경|
|시뮬레이션|비결정론적 (예측 기반)|결정론적 (입력 기반)|
|클라 동작|즉시 반응, 서버 결과로 보정	예상|상태 유지, 과거로 롤백 후 재계산|
|적합 장르|협동 게임, 액션 RPG 등|격투, RTS, 대전 FPS 등|

|개발 난이도|상대적으로 낮음|높음 (결정론 요구)|


## 퀀텀 문제? 점

각 클라이언트는 아래와 같이 예측해서 

각 유저가 동시에 출발 때 실제로는 같은 위치에 있지만 클라이언트 예측으로 자신이 먼저 출발한 것으로 보인다

또한 동시에 멈출 때는 자신은 즉시 멈추지만 상대 유저는 앞으로 이동하고 있다고 예측 시뮬레이션 하기 때문에 더 앞으로 가다가 돌아온다.

그래서 아이템 같이 동시에 처리되면 안되는 것은 아래 코드로 검증된 프레임에서 처리한다

``` c#
if (f.IsVerified)
{
    // 실행 코드
}
```

하지만 단순 이펙트나 사운드는 예측 프레임에서 처리해도 무방하다.

https://github.com/user-attachments/assets/fd6b2781-f781-45f1-89a9-e51c82236c71


## 포톤 퓨전 튜토 정리 (미완)
```
결국 서버 프로그래밍의 핵심은
1. 서버와의 딜레이를 어떤식으로 줄이고
2. 아무리 줄여도 남아있는 딜레이를 어떻게 처리하냐

A. 클라우드 게임 방식 (클라이언트에서는 입력을 보내면 모든 처리를 서버에서하고 결과면 클라에서 출력)
B. 예측 기반 처리 (앞으로 가고 있으면 계속 앞으로 간다고 생각하고 처리 후 실제로 앞으로 가고 있으면 그대로 멈추면 추가로 이동한만큼 돌리기)
C. 특별한 처리를 하지 않고 딜레이된 속도로 이동

#포톤 퓨전 튜토

## 오브젝트 생성
Runner.Spawn(PlayerPrefab) 를 한명만 호출해도
모든 클라이언트에 생성됨 (네트워크 오브젝트 컴포넌트가 붙은 오브젝트만 가능)

NetworkObject: 존재, 삭제 공유됨

Runner.Spawn 는 SimulationBehaviour 를 상속 받아야 호출 가능
+ SimulationBehaviour 는 Network Runner와 같은 오브젝트에 붙여야됨
=> 왜냐면 NetworkRunner 가 시작 시 자신&하위에 있는 SimulationBehaviour 를 등록

IPlayerJoined 를 붙여서 PlayerJoined 이벤트를 구현하거나
FixedUpdateNetwork 에서 동작해서 Runner가 null이 아니다

FixedUpdateNetwork 에서 Input.GetKeyDown 은 정상 작동하지 않음
Update문에서 Input flag를 설정해서 FixedUpdateNetwork 에서 처리하는 방식으로 구현 가능


서로 다른 클라이언트 버전일 경우 문제 (한쪽에만 id가 추가됨)


포톤 오브젝트가 붙은 오브젝트 = 포지션 공유?
```

## 퀀텀 정리 (미완)

```
퓨전은
러너를 기준으로 생성 등을 하고
생성된 오브젝트는 authority 를 가진사람 (생성자) 가 권한을 가지고 업데이트를 할 수 있다.
NetworkObject로 설정하면 퓨전에서 동기화 해준다. NetTransform, [Network] 속성
[Networked] 는 아무곳에서나 수정하면 나중에 알아서 동기화 된다
네트워크 관련 함수에서만 NetworkObject를 변경할 수 있다 (transform 등) / 권한이 있는 사람만

Update 등 유니티 라이프사이클 함수에서는 Runner 가 null 일 수도 있다 (Runner 초기화 전에 먼저 호출될 경우)

FixedUpdateNetwork 에서는 매 프레임 불리는 게 아니라서 Input.GetKeyDown(KeyCode.P) 가 무시될 수 있다

RPC 를 이용해 모두에게 적용되는 이벤트 호출 (이건 호출 권한만 있으면 유니티 함수에서도 호출 가능)

host는 host가 모든 오브젝트를 만들고 움직인다 / host가 나가면 방 종료
shared는 각자가 오브젝트 스폰이 가능하고 각자 조종이 가능함, 공용 오브젝트 (보스) 는 master 라는 대표(먼저 들어온사람)이 생성하고 관리하도록 설정, 마스터가 나가면 다음 마스터가 처리

퀀텀과 퓨전은 프리딕트? 선예측 후 보정이라는 큰 개념에서 같으나

이걸 구현하는 방식과 결과물이 아주 다르다
뭐가 가장 큰 차이냐면

퓨전은 상태 동기화
- 점프를 누르면 점프하는 y좌표를 실시간으로 동기화
퀀텀은 입력 동기화
- 점프라는 행동만 동기화해서 각자 클라이언트에서 똑같이 점프가됨?
(나중에 휴대폰으로 vpn키고 느린 환경에서 테스트)

==============================================================

[퀀텀]
ECS 최적화를 위해 시스템 채택 (네트워크랑 큰 관련 없이)
입력 동기화 + 결정론적 시스템이 핵심

기본 유니티는 E (C+S) 구조라고 생각하면 편함
오브젝트 / 스크립트

ECS는 
오브젝트 / 변수 선언 / 변수가 없는 스크립트
이렇게 나뉜다

컴포넌트가 변수와 태그 역할을 수행해서
시스템은 모든 오브젝트에 적용된다 (원하는 컴포넌트가 붙은 오브젝트만 필터링 가능)
시스템 끼리 통신은 시그널 시스템 이용, 아마 옵저버/이벤트 버스 개념

뭐 하나 만드려면
오브젝트 / 시스템 / qtn
이렇게 최소한 3개 만들고
오브젝트에는 Quantum Entity Prototype 붙이고
Quantum Entity Prototype 에 qtn 붙이고
시스템에서는 만든 qtn 필터 걸어서 스크립트 작성

이렇게 해야하는듯

[퀀텀 기본 구조]
퀀텀은 씬 구성은
기본적으로 
- QuantumEntityViewUpdater - 씬의 엔티티를 업데이트 해주는?
- QuantumDebugRunner (메뉴를 통해 입장하면 비활성화 되고 QuantumRunner 가 생성됨)
- QuantumMap - 맵에 미리 배치된 오브젝트 (벽, 장애물) 을 굽는다?
이렇게 필수 구성

제일 중요한 건 Runner로 여기에 모든 걸 등록해야 한다.
- Map config : 
- Simulation config : 글로벌 물리 설정 (중력 등)
- System config : 이 씬에서 동작할 System 스크립트가 모여있는 설정

스크립트
- System : SystemSignalsOnly 혹은 SystemMainThreadFilter 를 상속받아야 한다.
SystemMainThreadFilter 는 필터로 설정한 컴포넌트가 있는 엔티티만 방문한다.
SystemSignalsOnly 는 이벤트로만 호출되는 함수만 있는 스크립트다

- Component : qtn 파일로 변수, 이벤트 선언만 가능하다
시스템에서 필터를 걸 때 많이 사용될듯?

- Entity : Component 가 붙는 객체
퀀텀 오브젝트를 만들면 있는 Quantum Entity Prototype 밑에 컴포넌트 추가 가능

==========================================
system 은 순수 C# 스크립트로 

view 스크립트는 유니티 
input은 view로 그냥 일반 MonoBehaviour 에 
QuantumCallback.Subscribe 걸면됨

폴더 나누기는 필수인가?
-> ㄴㄴ 개념상 그냥 그런 느낌
```
