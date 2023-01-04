---
title: "heapSort"
date: 2023-01-03
draft: false
category: [study]
subcategories: [Algorithm]
tags: [Algorithm]
---


`병합정렬, 퀵정렬, 힙정렬`에 대해 정리해보도록하겠다.
단순정렬의 시간복잡도는 삽입정렬을 제외한 모든정렬이 O(N^2)이다. 하지만 지금부터 볼 병합정렬과 퀵정렬 힙정렬은 모두 시간복잡도가 단순정렬보다 작다.

<!--more-->

힙정렬은 평균적인 시간복잡도와 최악의 경우 시간복잡도가 `O(nlogn)`으로 병합정렬과 동일하다. 만약 100개의 데이터중 20번째로 작은 값을 찾아야 할때 가장 효율적인 정렬에 해당한다. 다른 정렬알고리즘들은 모든 정렬을 끝내야 20번째로 작은 값을 찾을 수 있지만, 힙정렬의경우 20번의 정렬만으로 20번째로 작은 값을 찾을 수 있기 때문이다.

힙정렬은 이진탐색트리를 기본으로 정렬이 이루어진다. `초기힙만들기`과 `힙정렬`두 단계로 진행이 되는데, 먼저 `초기힙만들기`부분부터 그림으로 살펴 보겠다.

![](/images/study/Algorithms/heapSort/1.JPG)
![](/images/study/Algorithms/heapSort/2.JPG)
![](/images/study/Algorithms/heapSort/3.JPG)

초기힙을만드는 부분은 그림으로 이렇게 그릴 수 있다. 내가 그린그림은 최소힙에 해당한다. 설명하자면 아무렇게나 나열되어있는 7 / 9 / 4 / 8 / 6 / 3 배열을 트리의 형태로 바꾸는 단계이다. 초기힙에서 중요한 사항은 트리의 가장 최하위 레벨부터 진행해야 한다는것이다. 이진탐색트리의 성질을 이용해 자식중 더 작은값을 부모와 비교해 만약 부모보다 자식이 더 작다면 부모와 자리를 스왑하는 방법이다. 부모와 자식 자리사이에 스왑이 일어나고 나면 또다시 그 아래자식과 한번더 비교를 한번 더 진행해주어야한다.

![](/images/study/Algorithms/heapSort/4.JPG)
![](/images/study/Algorithms/heapSort/5.JPG)
![](/images/study/Algorithms/heapSort/6.JPG)
![](/images/study/Algorithms/heapSort/7.JPG)
![](/images/study/Algorithms/heapSort/8.JPG)
![](/images/study/Algorithms/heapSort/9.JPG)

초기힙이 만들어졌다면 힙정렬의 준비가 끝난것이다. 힙정렬은 위의 사진처럼 진행이 된다. 이진탐색트리의 삭제와 동일한 방법으로 진행이 된다. 루트노드가 삭제되면 가장 마지막에 위치한 노드가 루트노드로 올라와 다시 힙을 만들어 나가는 과정을 반복하며 정렬이 이루어진다.

```C
void heapify(int* arr, int k, int len) {
    int left = 2 * k, right = (2 * k) + 1;
    int larger;
    if (right <= len) { 
        if (arr[left] > arr[right]) {
            larger = left;
        }
        else larger = right;
    }
    else if (left <= len) {
        larger = left;
    }
    else return; 
    if (arr[larger] > arr[k]) {
        int tmp = arr[larger];
        arr[larger] = arr[k];
        arr[k] = tmp;
        heapify(arr, larger, len);
    }
}

void buildHeap(int* arr, int len) {
    for (int i = len / 2; i >= 1; i--) {
        heapify(arr, i, len);
    }
}

void heapSort(int* arr, int len) {
    buildHeap(arr, len); 
    for (int i = len; i >= 2; i--) {
        printf("[%d, %d] swap\n", arr[1], arr[i]);                  
        int tmp = arr[1];
        arr[1] = arr[i];
        arr[i] = tmp;
        heapify(arr, 1, i - 1);
    }
}
```