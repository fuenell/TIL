# Burst Compiler
Burst Compiler는 Unity가 제공하는 고성능 컴파일러로, C# 코드를 네이티브 코드로 변환하며 최적화를 적용합니다. **SIMD(Vectorization)** 와 같은 고급 최적화 기술을 활용해 반복 연산이나 수학 계산을 빠르게 처리합니다.

- SIMD: 여러 데이터에 대해 동시에 연산하여 처리 속도를 높이는 방식입니다.
- 벡터화: 코드를 최적화해 CPU에서 병렬 처리가 가능하게 합니다.

## Burst와 IL2CPP의 관계
IL2CPP는 C# 코드를 C++로 변환해 네이티브 바이너리를 생성하고, Burst는 이 네이티브 코드를 더 최적화합니다. 두 가지를 함께 사용하면 IL2CPP로 기본 성능을 확보한 후, Burst의 고급 최적화로 추가적인 성능 향상을 얻을 수 있습니다.

## Burst Compiler의 장점과 단점
### 장점
- 성능 최적화: 고속 연산이 필요한 코드에서 큰 성능 향상을 제공합니다.
- 다중 플랫폼 지원: 다양한 플랫폼에서 최적화된 네이티브 코드를 생성합니다.

### 단점
- 빌드 시간 증가: 최적화 과정에서 빌드 시간이 늘어날 수 있습니다. (※진짜 느려지는지 검증 필요※)
- 디버깅 어려움: 최적화로 인해 코드 구조가 변경되어 디버깅이 어려울 수 있습니다.

## Burst Compiler 설정 방법
1. Jobs -> Burst -> Enable Compilation 옵션을 켜면 Burst를 활성화할 수 있습니다.
2. 성능이 중요한 코드에 Burst를 적용하여 빌드 시간을 줄입니다. (코드 직접 수정 필요)
3. Jobs -> Burst -> Open Inspector 창에서 최적화된 코드를 확인할 수 있습니다.

## 결론
Burst Compiler는 특히 성능이 중요한 Unity 프로젝트에서 효과적입니다. IL2CPP와 함께 사용할 경우 성능을 극대화할 수 있으며, 성능과 빌드 시간을 고려해 필요한 부분에만 적용하는 것이 좋습니다.