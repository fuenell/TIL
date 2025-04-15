# External Dependency Manager for Unity(EDM4U)


### Unity Package Manager의 종속성
유니티 패키지 매니저의 종속성 처리
패키지 별로 dependencies(종속) 패키지가 있을 수 있음
내가 사용하는 패키지가 (A, B일 경우)
A(1.0) -> T(2.0) 를 종속하고 있고
B(3.1) -> Y(4.8) -> T(2.1) 를 종속하고 있다면
T는 base 패키지(A,B)에 가까운 버전인 T(2.0) 버전만 사용

Packages/[패키지명]/package.json

"dependencies": {
  "com.google.android.appbundle": "1.9.0",
  "com.google.play.common": "1.9.2",
  "com.google.play.core": "1.8.5"
},


### EDM4U
EDM4U 으로 종속성 처리
Resovle 혹은 빌드 시 처리됨

Packages/[패키지명]/Editor/Dependencies.xml

<dependencies>
  <androidPackages>
    <androidPackage spec="com.google.android.gms:play-services-ads:23.2.0">
      <repositories>
        <repository>https://maven.google.com/</repository>
      </repositories>
    </androidPackage>

  <iosPods>
    <iosPod name="Google-Mobile-Ads-SDK" version="~> 11.7.0">
      <sources>
        <source>https://github.com/CocoaPods/Specs</source>
      </sources>
    </iosPod>
  </iosPods>
</dependencies>



유니티 android 빌드 시


![EDM4U](https://github.com/user-attachments/assets/7cbd8b84-a9c3-427c-b015-b6187b66263a)