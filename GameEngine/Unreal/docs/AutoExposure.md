# 자동 노출 보정 (암순응)
언리얼에는 기본적으로 자동 노출 보정 기능이 켜져있다

어두운 곳에서 계속 있으면 눈이 적응해서 점점 보이는 것 (암순응)
효과를 구현한 것 같음

### 왜 PP가 없어도 자동 노출이 되나요?

- **PP 볼륨이 없으면** 엔진은 **Project Settings > Rendering > Default Settings**에 있는 **기본 렌더/포스트프로세스 설정으로 동작**합니다.

## EV 설명

EV = Exposure Value(노출 값)

그런데 계산식이 `Exposure = 1 / (2^EV)`

이기 때문에 EV가 높아지면 어둡고

EV가 낮아지면 밝아진다

- **Min EV100** = “아무리 어두워도, 여기까지만 밝게 해라” (밝게 하는 한계)
- **Max EV100** = “아무리 밝아도, 여기까지만 어둡게 해라” (어둡게 하는 한계)

<img width="1023" height="532" alt="Image" src="https://github.com/user-attachments/assets/e25dd5a1-3ed8-4126-bb02-70d354f7c395" />

# 해결법

머티리얼 그래프에서

EyeAdaptation / EyeAdaptationInverse 노드를 사용

EyeAdaptation 는 현재 적용되는 노출 보정량

EyeAdaptationInverse 는 `1/EyeAdaptation` 으로 노출 보정량을 상쇄 가능

아래 계산식으로 노출 보정을 무시하고 표시 가능

`Emissive = AuroraBase * Intensity * EyeAdaptationInverse`

하지만 월드가 밝을 때도 똑같은 밝기로 보이기 때문에 어색하게 보일 수 있다 (ex. 대낮에 오로라가 잘 보임)

그럴 때는`EyeAdaptationInverse`에 0.5 정도를 곱한 후 사용해서

자동 노출의 영향을 절반만 적용받게도 가능하다