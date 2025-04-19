# 오브젝트 풀 패턴 (Object Pool Pattern) - for Unity
오브젝트를 미리 생성해둔 뒤 새로운 오브젝트가 필요할 때 미리 생성해둔 오브젝트를 사용하고 해제하여

런타임 생성/삭제 성능을 개선 시키는 방법

# 간이 풀링 방법
간이 풀링 방법으로 최대로 생성될 개수를 미리 알 수 있다면 그것보다 많이 생성해두면 굳이 해제를 일일이 체크하지 않고 계속 사이클을 돌릴 수 있다. (ex.10*10 오목판에 생성될 수 있는 최대 돌은 100개), 주로 프로토타이핑

``` c#
public class BlockPoolingManager : MonoBehaviour
{
    [SerializeField]
    private int _maxPoolingCount = 60;

    [SerializeField]
    private Block _blockPrefabs;
    private Block[] _poolingBlocks;
    private int _poolingIndex;

    private void Awake()
    {
        _poolingIndex = 0;
        _poolingBlocks = new Block[_maxPoolingCount];
        for (int i = 0; i < _maxPoolingCount; i++)
        {
            Block newBlock = Instantiate(_blockPrefabs, this.transform);
            newBlock.gameObject.SetActive(false);
            _poolingBlocks[i] = newBlock;
        }
    }

    public Block GetBlock()
    {
        Block block = _poolingBlocks[_poolingIndex];
        _poolingIndex = (_poolingIndex + 1) % _poolingBlocks.Length;
        return block;
    }
}
```

# 정규적인 방법
할당과 해제를 사용하여 모든 오브젝트가 할당된 상태면 추가로 오브젝트를 생성해서 리스트에 추가시켜야 한다. / 또한 목표 유지량 보다 많으면 알아서 삭제 시켜야 한다.

직접 구현해도 되지만 Unity에서 이미 제공해주는 인터페이스를 활용하면 빠르게 구현이 가능하다.

``` c#
// 오브젝트가 풀로 반환될 때 실행될 액션을 설정할 수 있도록 인터페이스 정의
public interface IReleasable
{
    void SetReleaseAction(UnityAction releaseAction);
}

// 제너릭 오브젝트 풀링 매니저 클래스
public class ObjectPoolingManager<T> where T : MonoBehaviour, IReleasable
{
    private T _obejctPrefab;        // 풀링할 프리팹 오브젝트
    private int _initialPoolSize;   // 풀의 초기 용량 (스택 크기)
    private int _maxPoolSize;       // 풀에 저장 가능한 최대 오브젝트 수

    private IObjectPool<T> _pool;   // 실제 오브젝트 풀

    // 생성자
    public ObjectPoolingManager(T prefab, int initialPoolSize = 10, int maxPoolSize = 10)
    {
        _obejctPrefab = prefab;
        _initialPoolSize = initialPoolSize;
        _maxPoolSize = maxPoolSize;

        // 풀 내부 동작 정의
        _pool = new ObjectPool<T>(
            CreatedPooledItem,     // 새 오브젝트 생성 시 호출
            OnTakeFromPool,        // 오브젝트를 풀에서 꺼낼 때 호출
            OnReturnedToPool,      // 오브젝트를 풀에 반납할 때 호출
            OnDestroyPoolObject,   // 풀 크기 초과 시 오브젝트 제거할 때 호출
            true,                  // 중복 Release 감지 여부 (true면 같은 오브젝트를 두 번 Release하면 에러 발생)
            _initialPoolSize,      // 풀 내부 스택의 초기 용량 (오브젝트가 미리 생성되는 건 아님, 단순 배열 크기 확보)
            _maxPoolSize           // 풀에 저장 가능한 최대 오브젝트 수 (초과하면 Release 시 오브젝트는 파괴됨)
        );
    }

    // 외부에서 오브젝트를 가져갈 때 사용하는 메서드
    public T GetObject()
    {
        return _pool.Get(); // 풀에서 하나 꺼냄 (없으면 새로 생성)
    }

    // 풀에서 사용할 새 오브젝트를 생성하는 메서드 (pool에서 꺼낼 오브젝트가 없으면 호출됨)
    private T CreatedPooledItem()
    {
        T obj = UnityEngine.Object.Instantiate(_obejctPrefab); // 프리팹으로 새 오브젝트 생성
        obj.name = typeof(T).Name; // 이름 지정 (디버깅용)

        // 해당 오브젝트에 Release 액션 설정: 나중에 반납할 때 사용
        obj.SetReleaseAction(() => _pool.Release(obj));

        return obj;
    }

    // 오브젝트가 풀에 반납될 때 호출되는 메서드 (pool.Release(obj)를 호출해주어야 한다)
    private void OnReturnedToPool(T obj)
    {
        obj.gameObject.SetActive(false); // 비활성화 처리
    }

    // 오브젝트를 풀에서 꺼낼 때 호출되는 메서드 (obj는 pool 목록에서 제외된 상태가 됨)
    private void OnTakeFromPool(T obj)
    {
        obj.gameObject.SetActive(true); // 활성화 처리
    }

    // 풀 최대치를 초과해서 생성된 오브젝트는 제거할 때 호출되는 메서드
    private void OnDestroyPoolObject(T obj)
    {
        GameObject.Destroy(obj.gameObject); // 완전히 파괴
    }
}
```
