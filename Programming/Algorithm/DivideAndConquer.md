# 분할정복 (divide-and-conquer)
주어진 문제를 더 이상 나눌 수 없을 때까지 작은 문제들로 순환적으로 분할하고, 각각 정복(해결) 후 결합을 통해 문제의 해를 구하는 설계 방식

대표적으로 분할정복 기법이 사용된 알고리즘은 이진 탐색, 퀵 정렬, 합병 정렬 등이 있다

##  이진 탐색

정렬된 데이터를 절반씩 분할하며 데이터를 탐색하는 알고리즘

```C#
int BinarySearch(int[] data, int left, int right, int target)
{
    if (left > right)
        return -1;
    int mid = (left + right) / 2;

    if (target == data[mid])
        return mid;
    else if (target < data[mid])
        return BinarySearch(data, left, mid - 1, target);
    else
        return BinarySearch(data, mid + 1, right, target);
}
```

이진 탐색의 성능은 점화식을 통해 구할 수 있다

```
T(n) = 데이터의 수가 n일 때 일어나는 모든 비교 횟수의 합
T(n) = T(n/2) + O(1)
T(n) = O(logn)
```

## 퀵 정렬

특정 원소를 기준으로 배열을 2분할하고 각 부분배열에 대해서 퀵 정렬을 순환적으로 적용해 정렬하는 알고리즘

#### 알고리즘 순서

1. 특정 원소를 피벗(기준)으로 지정한다
2. 피벗보다 작은 수는 앞으로 보내고 큰 수는 피벗 뒤로 보낸다
3. 피벗을 기준으로 좌측과 우측 배열에 퀵 정렬을 적용한다
4. 퀵 정렬을 실행하는 배열의 길이가 1이 될 때까지 반복한다

```C#
void QuickSort(int[] data, int left, int right)
{
    if (right - left < 1)	// 배열의 길이가 1이 되면 종료한다
        return;

    int newPivotIndex = Partition(data, left, right);	// 배열을 피벗을 기준으로 나눠 정렬한다

    QuickSort(data, left, newPivotIndex - 1);	// 좌측 배열에 퀵 정렬을 실행한다
    QuickSort(data, newPivotIndex + 1, right);	// 우측 배열에 퀵 정렬을 실행한다
}
```

퀵 정렬의 성능 역시 점화식을 통해 구할 수 있다

```
최악의 수행시간 (피벗으로 배열을 나눴을 때 n-1과 1로 나눠는 경우)
T(n) = T(n-1) + T(0) + O(n)
T(n) = O(n^2)
```

```
최선의 수행시간 (피벗으로 배열을 나눴을 때 절반으로 나눠는 경우)
T(n) = 2T(n/2) + O(n)
T(n) = O(nlogn)
```

정렬된 데이터에서 첫 번째 원소를 피벗으로 지정할 경우 시간 복잡도는 최악의 수행시간인 O(n^2)이다

그러나 피벗을 랜덤하게 잡거나 데이터가 정렬되지 않은 경우 평균적으로 O(nlogn)의 시간 복잡도를 가진다