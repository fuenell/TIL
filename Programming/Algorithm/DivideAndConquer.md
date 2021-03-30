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

## 합병 정렬

주어진 배열을 각 원소가 하나가 될 때 까지 동일한 크기의 두개의 부분배열로 분할하고, 각 부분배열을 정렬하며 합병해 정렬된 배열을 만드는 정렬 방식

합병정렬의 최악의 시간복잡도가 `O(nlogn)`이다

그러나 단점으로 입력크기 n만큼의 추가적인 메모리가 필요하다, 즉 제자리 정렬 알고리즘이 아니다

```C#
void MergeSort(int[] data, int n)
{
    if (n > 1)	// 배열의 길이가 1보다 크면 분할한다
    {
        int mid = n/2;
        
        int[] left = MergeSort(data[0 ~ mid-1], mid);
		int[] right = MergeSort(data[mid ~ n-1], mid);
        
        data = Merge(left, right);		// 정렬하며 합병한다
    }
    return data;
}
```

## 선택 문제

n개의 원소가 임의의 순서로 저장된 배열에서 i번째로 작은 원소를 찾는 문제

#### 해결 방법

1. 최소값을 찾는 과정을 i번 반복 `O(ni) = O(n²)`

2. 오름차순으로 정렬한 후 i번째 원소를 찾는 방법 `O(nlogn)`

   2-1. 퀵 정렬 방식으로 피벗으로 배열을 분할하며 i번째 원소를 바로 찾는 방법 `최악 O(n²), 평균 O(n) `

3. 중간값들의 중간값을 구해 피벗으로 사용해 i번째 원소를 찾는 방법 `O(n)`