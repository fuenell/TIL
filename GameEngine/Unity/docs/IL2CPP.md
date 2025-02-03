# IL2CPP [#Unity Tips](https://youtu.be/-9X965jXrn8?si=eHGXb6IG6Z-sxQq6)

# 미완


il2cpp 설정을 하면 빌드가 엄청 느려짐 왜 써야하지?
- ARM-64비트를 지원하려고 (필수)
- 더 빠른 성능 (런타임 컴파일이 없어짐)

### [Mono - JIT(just in time)] 방식
c#은 빌드 시 최종적으로 mono 위에서 돌아간다

C# -> 빌드 시 -> IL(.net 어셈블리) -> 런타임 시 il을 mono가 컴파일해서 os위에 올림

### [il2cpp - AOT] 방식
C# -> il -> cpp -> 네이티브

제네릭을 cpp로 풀어쓰면 사용하는 타입별로 다 선언됨
faster builds하면 그 부분을 공유 코드로 사용 대신 타입 검사를 런타임에 하기 때문에 약간 성능 저하

## 관련 빌드 옵션

### li2cpp code generation
- Faster runtime: 런타임 성능에 최적화된 코드를 생성합니다.
- Faster (smaller) builds: 빌드 크기와 반복에 최적화된 코드를 생성해서 용량이 줄어들지만 성능은 저하될 수 있습니다.

### C++ compiler
- debug: 최적화를 모두 해제하여 빌드 속도는 빠르지만 실행 성능은 낮습니다.
- release: 최적화를 적용하여 실행 성능이 향상되고 바이너리 크기가 줄어들지만, 빌드 시간은 길어집니다.
- master: 가능한 모든 최적화를 수행하여 최대 성능을 내지만, 빌드 시간이 Release보다 훨씬 더 오래 걸립니다. (출시 버전 빌드 시 이 구성을 권장)

### CompressionMethod
LZ4: 용량 조금 크기만 빨리 빌드됨
LZ4HC: 빌드가 오래 걸리지만 더 높은 압축률 (용량 줄어듬)
