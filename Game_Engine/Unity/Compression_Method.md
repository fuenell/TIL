# Compression Method (압축 방식)
유니티 빌드시 압축 방식을 설정해 용량을 줄일 수 있다

Default - 기본 압축 포맷입니다. ZIP는 LZ4 및 LZ4HC보다 압축률은 조금 더 뛰어나지만 데이터의 압축을 푸는 속도가 느립니다.
LZ4 - 개발용 빌드에 적합한 고속 압축 포맷입니다. LZ4 압축을 사용하면 Unity에서 빌드된 게임과 앱의 로딩 시간이 크게 향상됩니다. 자세한 내용은 BuildOptions.CompressWithLz4를 참조하십시오.
LZ4HC - 높은 압축률을 자랑하는 LZ4 변형 포맷입니다. LZ4HC는 빌드 속도가 느리지만 릴리스 빌드에서 더 뛰어난 결과를 제공합니다. LZ4HC 압축을 사용하면 Unity에서 빌드된 게임과 앱의 로딩 시간이 크게 향상됩니다. 자세한 내용은 BuildOptions.CompressWithLz4HC를 참조하십시오.
