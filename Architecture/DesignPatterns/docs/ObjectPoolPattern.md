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
using UnityEngine;
using UnityEngine.Events;
using UnityEngine.Pool;

public interface IReleasable
{
    void SetReleaseAction(UnityAction releaseAction);
}

public class ObjectPoolingManager<T> where T : MonoBehaviour, IReleasable
{
    private int _maxPoolSize;
    private int _stackDefaultCapacity;
    private T _obejctPrefab;

    private IObjectPool<T> _pool;

    public IObjectPool<T> Pool
    {
        get
        {
            if (_pool == null)
                _pool =
                    new ObjectPool<T>(
                        CreatedPooledItem,
                        OnTakeFromPool,
                        OnReturnedToPool,
                        OnDestroyPoolObject,
                        true,
                        _stackDefaultCapacity,
                        _maxPoolSize);
            return _pool;
        }
    }

    public ObjectPoolingManager(T prefab, int maxPoolSize = 10, int stackDefaultCapacity = 10)
    {
        _obejctPrefab = prefab;
        _maxPoolSize = maxPoolSize;
        _stackDefaultCapacity = stackDefaultCapacity;
    }

    // 오브젝트 생성
    private T CreatedPooledItem()
    {
        var obj = UnityEngine.Object.Instantiate(_obejctPrefab);
        obj.name = typeof(T).Name;
        obj.SetReleaseAction(() => Pool.Release(obj));

        return obj;
    }

    // 오브젝트가 반환 시
    private void OnReturnedToPool(T obj)
    {
        obj.gameObject.SetActive(false);
    }

    // 오브젝트 요청 시
    private void OnTakeFromPool(T obj)
    {
        obj.gameObject.SetActive(true);
    }

    private void OnDestroyPoolObject(T obj)
    {
        GameObject.Destroy(obj.gameObject);
    }

    public void Spawn()
    {
        var amount = Random.Range(1, 10);

        for (int i = 0; i < amount; ++i)
        {
            var obj = Pool.Get();

            obj.transform.position = Random.insideUnitSphere * 10;
        }
    }
}
```
