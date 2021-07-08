# 2D / 3D 모드 설정 [#Manual](https://docs.unity3d.com/Manual/2DAnd3DModeSettings.html)
Project Settings에서 2D와 3D 모드를 설정하면 해당 모드에서 사용하는 여러 옵션들이 미리 설정된다  
(프로젝트 생성 시 2D, 3D를 선택하면 자동으로 설정된다)
ex) 2D모드에서 이미지를 Import하면 TextureType이 자동으로 Sprite로 설정된다  

## 설정 방법
Edit > Project Settings > Editor > Default Behaviour Mode > Mode  
![defulat_mode](https://user-images.githubusercontent.com/37904040/124854996-3bdf6b00-dfe3-11eb-97c6-e46122a5418a.jpg)

## 변경되는 옵션
#### 2D
- 이미지를 Import하면 TextureType이 Sprite로 설정된다
- 새로운 씬 생성 시  
-> 카메라의 Projection이 Orthographic으로 설정된다  
-> Directional Light가 생성되지 않는다  

#### 3D
- 이미지를 Import하면 TextureType이 Default로 설정된다
- 새로운 씬 생성 시  
-> 카메라의 Projection이 Perspective으로 설정된다  
-> Directional Light가 생성된다
