# LightMode [#API](https://docs.unity3d.com/2020.3/Documentation/ScriptReference/Experimental.GlobalIllumination.LightMode.html)
빛 모드 설정
 
Light에는 여러가지 설정이 있다.  
Light 세부 옵션은 `Window > Rendering > Lighting` 메뉴에서 설정이 가능하다.

![image](https://user-images.githubusercontent.com/37904040/199873219-045cc996-4709-4f11-8fb1-fd53b3f24bde.png)

# RealTime
빛을 매 프레임마다 계산해 오브젝트가 움직이거나 빛의 각도가 변경되어도 실시간으로 반영된다.

# Mixed
Light 설정에서 Scene > Mixed Lighting > Lighting Mode 옵션에 따라 빛 모드를 설정할 수 있다.


> Baked Indirect

Global Illumination만 라이트맵에 Baked하고 그림자는 실시간으로 계산한다.

> Subtractive  

Static Object는 모두 Baked하고 그 외 오브젝트는 실시간으로 계산한다.

> Shadowmask

Static Object들의 그림자를 Shadowmask로 따로 만들어 사용한다.  
Camera 와 가까운 오브젝트는 Realtime 그림자를, 멀리있는 오브젝트는 Shadowmask 그림자를 렌더링한다.  
Project Settings > Quality > Shadows > Shadowmask Mode 에서 Distance Shadowmask 옵션 선택 시 활성화된다. (Shadow Distance로 거리 설정 가능)


# Baked
빛과 그림자를 미리 계산해 라이트 맵으로 만들어서 고정된 빛을 사용하는 방식이다.
하지만 static이 적용된 물체만 빛이 계산된다.  
