---
title: "binarySearch"
date: 2023-01-09
draft: false
category: [study]
subcategories: [Algorithm]
tags: [Algorithm]
---

`binary search` / 이분탐색은 정렬된 리스트를 절반씩 나눠가며 탐색하는 알고리즘이다.

<!--more-->

이진탐색의 시간복잡도는 평균 `O(logn)` 최악의 경우에도 `O(logn)`이다. 최선의 경우에는 찾고자 하는 데이터가 정 중앙에 있을때 1번의 탐색으로 찾을 수 있다.

이진 탐색의 점화식은 `T(n) = T(n/2) + 1`로 표현이 가능하다. 그림을 보며 점화식이 이렇게 유도되는 이유를 살펴보겠다.

![](/images/study/Algorithms/binarySearch/1.JPG)

그림과 동일하게 찾고자하는 데이터가 배열 mid인덱스의 데이터보다 크다면 mid를 start로 바꾸어 범위를 바꾸어준다. 이와 반대되는 경우는 반대로 mid를 end로 바꾸어 범위를 바꾸어 주면된다.

```c
int binarySearch(int data[], int n, int key) {
    int start, end;
    int mid;
 
    start = 0;
    end = n - 1;
    while (start <= end) {
        mid = (start + end) / 2;
        if (key == data[mid]) {
            return mid;        
        }
        else if (key < data[mid]) {
            end = mid - 1;
        }
        else if (key > data[mid]) {
            start = mid + 1;
        }
    }
    return -1;
}
```