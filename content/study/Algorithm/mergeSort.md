---
title: "mergeSort"
date: 2022-12-3
draft: false
category: [study]
subcategories: [Algorithm]
tags: [Algorithm]
---

#정렬

##병합정렬
`병합정렬, 퀵정렬, 힙정렬`에 대해 정리해보도록하겠다.
단순정렬의 시간복잡도는 삽입정렬을 제외한 모든정렬이 O(N^2)이다. 하지만 지금부터 볼 병합정렬과 퀵정렬 힙정렬은 모두 시간복잡도가 단순정렬보다 작다.

<!--more-->

우선 병합정렬부터 살펴보겠다. 병합정렬의 경우 반을 자르고 자르고 내려가 정렬하고자 하는 배열의 원소가 하나가 될 때까지 자르고 Merge과정에서 두 배열의 인덱스 값을 비교하며 붙여나가는 형태이다.

![](/images/study/Algorithms/mergeSort/1.png)

이런식으로 순환을 이용해 원소가 한 개가 될 때까지 반으로 나누어 나간다.

![](/images/study/Algorithms/mergeSort/2.png)
![](/images/study/Algorithms/mergeSort/3.png)
![](/images/study/Algorithms/mergeSort/4.png)
![](/images/study/Algorithms/mergeSort/5.png)
![](/images/study/Algorithms/mergeSort/6.png)

사진처럼 파란부분의 첫 번재 인덱스, 그리고 파란부분의 첫 번째 인덱스를 비교해 더작은값을 다른 배열에 저장시켜주는 방식이다. 병합정렬에서 중요한건 원래 배열이 저장되어있는 저장공간에서 새로운 배열을 만들어 옮기고 또다시 정렬된 배열을 원래 배열로 옮겨두어야 한다는 것이다. 그 이유는 순환이기 때문이다. 이제 코드를 살펴 보겠다.

```C
void Merge(int* arr, int left, int mid, int right, int len) {
    int i = left, j = mid + 1, t = 0;
    int* tmp = (int*)malloc(sizeof(int) * len);

    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) tmp[t++] = arr[i++];
        else tmp[t++] = arr[j++];
    }

    while (i <= mid) tmp[t++] = arr[i++];
    while (j <= right) tmp[t++] = arr[j++];

    i = left, t = 0;
    while (i <= right) arr[i++] = tmp[t++];

    PrintArray(arr, left, right, len);
    free(tmp);
}

void MergeSort(int* arr, int left, int right, int len) {
    int mid;
    if (left < right) {
        mid = (left + right) / 2;
        MergeSort(arr, left, mid, len);
        MergeSort(arr, mid + 1, right, len);
        Merge(arr, left, mid, right, len);
    }
}
```

Merge와 MergeSort두 부분으로 진행이 되는데, MergeSort는 제일 위에 보였던 사진처럼 배열을 계속해서 반으로 쪼개는 부분이고 Merge부분이 아래 값을 비교하며 새로운배열에 정렬해주는 형태를 나타낸 것이다.

이렇게 병합정렬의 시간복잡도는 `O(nlogn)`에 해당한다. 점화식으로는 `2T(n/2) + Q(n) (후처리식)`으로 표현할 수 있는데, 후처리식은 병합과정에 해당한다. 병합정렬은 일반적인경우, 평균의 경우, 최악의 경우에도 모두 O(nlogn)이라는 시간복잡도를 가진다.
