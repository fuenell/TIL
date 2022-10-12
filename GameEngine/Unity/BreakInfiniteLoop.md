# 무한루프 탈출 [#UnityBlog](https://blog.unity.com/community/breakout-how-to-stop-an-infinite-loop-in-a-unity-c-script)
반복문의 조건이 잘못되어 무한루프에 빠지는 경우가 있다.  
이럴 경우 일반 적으로 작업 관리자를 실행해 강제로 프로세스를 종료하는 방법을 사용한다.  
하지만 그럴 경우 저장되지 않은 작업이 손실될 수 있다.  
이럴 때 Visual Studio 의 프로세스 디버깅 기능을 이용하면 강제 종료를 하지 않고 무한루프를 빠져나올 수 있다.  
(반복문 코드에 따라 같은 방식으로 해결하지 못할 수 있다)

예를 들어 다음과 같은 코드가 실행되면 Unity Editor의 작동이 멈추면서 더 이상 작동되지 않는다.  
하지만 아래의 작업들을 진행한다면 루프를 넘어갈 수 있다.
``` c#
while (true)
{
    print("roop work");
}
```
## 무한루프 탈출 방법

먼저 Visual Studio를 열어 프로세스 디버깅 모드를 실행한다.

![image](https://user-images.githubusercontent.com/37904040/195285383-4b30901b-f11a-486e-a76c-6409b95c8cdc.png)

프로세스에서 Unity.exe - Game을 찾아 연결한다.

![image](https://user-images.githubusercontent.com/37904040/195285332-4137e597-2f70-441e-a942-91fec018c585.png)

[디버그] - [모두 중단] 메뉴 선택

![image](https://user-images.githubusercontent.com/37904040/195285747-43268ed4-ecaa-4d5a-a3dd-8dca7d717009.png)

[디버그] - [창] - [스레드] 메뉴 선택

![image](https://user-images.githubusercontent.com/37904040/195285540-9081e946-3133-4e88-8a54-fe41bbccf1fc.png)

스레드창에서 주스레드 선택

![image](https://user-images.githubusercontent.com/37904040/195285953-2b76b14d-b885-4d93-b38c-f44c9d38eddf.png)

호출 스택창에서 선택된 행 우클릭 후 외부 코드 표시 선택

![image](https://user-images.githubusercontent.com/37904040/195288876-85fe2b12-5c18-4fa9-8a8d-6a74410a9e0e.png)

화살표로 선택된 호출 스택 우클릭 후 디스어셈블리로 이동 선택

![image](https://user-images.githubusercontent.com/37904040/195289122-5e3cf383-31e8-4873-bd96-38a10d6b9cbd.png)

선택한다면 다음과 같은 창이 나타는데 이 부분이 중요하다.

![image](https://user-images.githubusercontent.com/37904040/195287560-eb29b267-d1ca-4a02-a22c-eed0de58d472.png)

F10을 꾹 눌러서 jmp 명령어를 기준으로 반복되는 블럭을 찾아낸다.

![image](https://user-images.githubusercontent.com/37904040/195293052-a8ace3d5-3552-4a88-8433-00d26c1d4f99.png)

반복되는 블럭을 찾았다면 jmp에 있는 화살표를 잡아서 한칸 밑으로 내린다.  
내린 후 Shift + F11 키를 꾹 눌러서 탈출한다.

![image](https://user-images.githubusercontent.com/37904040/195293071-431467ca-2b66-4476-8576-288d48fb5d51.png)

