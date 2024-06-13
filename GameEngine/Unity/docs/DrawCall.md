# Draw Call [#Unity Korea Youtube](https://www.youtube.com/live/UsyvT36vqpU?feature=share&t=1647)
드로우콜을 대략 설명하면 CPU에서 GPU로 오브젝트를 그려달라고 요청하는 것을 의미한다.  


## Render State
조금 디테일하게 설명한다면 GPU에는 VRAM이 있고 그곳에 오브젝트를 그릴 때 사용하는 리소스들이 있다.  
예를 들어 Mesh, Shader, Material, Texture 등이 있다.  
이때 GPU는 다음에 화면에 그려줄 오브젝트의 리소스들을 Render State에 저장한다.  
![image](https://github.com/normal111/TIL/assets/37904040/cd13672c-974c-493b-8f6d-c9fbcb3dfaba)

이때 드로우콜이 발생하면 현재 Render State에 있는 정보를 화면에 그려준다.  
그리고 다음 오브젝트를 그리려면 Render State를 수정하는 요청을 보내고 다음 드로우콜을 발생시킨다.  
이런 식으로 오브젝트를 모두 그리면 렌더 루프가 종료된다. (매 프레임 렌더 루프를 돈다)
![image](https://github.com/normal111/TIL/assets/37904040/e8b337dd-c5ab-4e7a-822f-8f0231e451c7)


## Set Pass Call
Render State를 수정하는 요청 중 Material 관련 정보를 수정하는 요청을 Set Pass Call이라고 한다.  
이러한 Set Pass Call은 비용이 큰편이라서 Set Pass Call만 줄여도 성능이 상승할 수 있다.  
Set Pass Call와 드로우콜을 합쳐서 넓은 의미로 드로우콜이라고 부르기도 한다. (Unity에서는 Batch)
![image](https://github.com/normal111/TIL/assets/37904040/2d477119-9eaa-4dfa-bf0c-ddc202e9e2d0)

같은 Material을 사용하는 오브젝트를 여러 개 그린다면 드로우콜은 늘어나지만 Set Pass Call은 늘어나지 않는다.
![image](https://github.com/normal111/TIL/assets/37904040/e76ea94e-252a-4410-9bf0-092eeca313e6)


## 드로우콜을 줄여야 하는 이유
드로우콜을 보낼 때 CPU는 GPU에서 사용하는 신호로 명령을 변환해 보내야 한다.  
이 변환 작업이 오버헤드가 있기 때문에 CPU에서 성능이 떨어질 수 있다.

정리하면 드로우콜이 많아지면 CPU가 할일이 많아져서 성능이 떨어진다.


## 드로우콜을 줄이는 방법
기본적으로 씬에 그려야하는 오브젝트의 개수가 줄어들어야 드로우콜을 줄일 수 있다.  
아니면 여러 오브젝트의 Mesh나 Material를 물리적으로 하나로 합쳐도 드로우콜을 줄일 수 있다.  
하지만 오브젝트를 건들 수 없는 경우에도 드로우콜을 줄이는 방법이 있다.  

1. Static Batching  
배경 오브젝트 같이 움직이지 않는 오브젝트를 Static으로 설정하면 하나의 오브젝트로 묶어서 한번에 그리는 기능이 있다.
`Project Settings > Player > Other Settings > Static Batching`으로 활성화해야 작동하다.

2. Dynamic Batching  
같은 Material을 공유하는 등 특정 조건이 맞는 오브젝트를 동적으로 합쳐서 한번에 그리는 기능이다.  
하지만 실시간으로 Mesh를 합치는 연산을 수행하기 때문에 오히려 성능이 떨어질 수도 있다.  
`Project Settings > Player > Other Settings > Dynamic Batching`으로 활성화하면 작동하다.

3. Sprite Atlas  
Sprite Atlas 에셋을 만들어서 여러 Sprite를 묶으면 하나의 Texture로 인식해 드로우콜을 줄일 수 있다.  
`Assets > Crate > 2D > Sprite Atlas`으로 에셋을 만들 수 있다.


## 드로우콜을 에디터에서 확인
1. Stats  
Game 창 우상단에 Stats 버튼을 눌러 현재 프레임과 드로우콜을 확인할 수 있다.  
아래의 경우 현재 Batches가 11이고 SetPassCall은 7이다.
![image](https://github.com/normal111/TIL/assets/37904040/bd4d7174-d744-4c16-ae76-7c3adf6ef5d8)

2. Frame Debug  
`Window > Analysis > Frame Debugger`으로 Frame Debug창을 열 수 있다.  
RenderLoopJob 위주로 확인하면 된다.  
![image](https://github.com/normal111/TIL/assets/37904040/ebfdc53d-9f98-408b-872a-0820ce75ac03)
