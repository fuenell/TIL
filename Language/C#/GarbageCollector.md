# GC(Garbage Collector) [#Docs](https://learn.microsoft.com/ko-kr/dotnet/standard/garbage-collection/fundamentals)
가비지 컬렉터는 힙 영역 메모리에 할당되어 있지만 사용하지 않는 메모리 영역을 자동으로 정리해주는 프로그램이다.  
GC를 사용한다는 것이 C#이 C++과 같이 메모리 할당과 해제를 직접해주는 언어와 큰 차이가 있는 부분이다.

# 작동 방식
가비지 수집기는 애플리케이션의 Root를 검사하여 더 이상 사용되지 않는 개체를 결정한다.  
Root는 전역변수, 남아있는 지역변수 등을 말하며 GC 수행 시 Root에서 접근할 수 없는 힙 영역을 초기화 시킨다.

# 가비지 수집 조건
1. 시스템의 실제 메모리가 부족할 때
2. 힙에 할당된 개체에 사용되는 메모리가 허용되는 임계값을 초과할 때
3. GC.Collect 메서드가 호출될 때
