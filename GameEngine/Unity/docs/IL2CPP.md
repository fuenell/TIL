# IL2CPP [#Unity Tips](https://youtu.be/-9X965jXrn8?si=eHGXb6IG6Z-sxQq6)
C# 프로그램을 구동하려면  다른 OS에 구동시키려면 다른 언어로 변환해야 한다.

그래서 유니티는 Mono 

IP2CPP는 

(정리 중)

li2cpp code generation
- faster builds: 빌드도 빠르고 용량도 작다
- runtime: 빌드도 느리고 용량도 크지만 빠르다?

C++ compiler
- debug: 용량 크지만 빠르게 빌드됨
- release: 빌드 오래 걸리지만 많이 압축됨
- master: release + 모든 최적화 수행

CompressionMethod
LZ4: 용량 적고 빨리 빌드됨
LZ4HC: 오래 걸리고 빠름, 더 높은 압축(용량 줄어듬)

위가 용량은 크지만 빠른 빌드
========================


구글 PAD를 사용하면된다
어드레서블 타겟을 pad에 사용할 수 있게 설정하는 작업이 필요함

그냥 스플릿 어플리케이션 바이너리를 사용하면 된다는 이야기가 있음, 근데 오류남
