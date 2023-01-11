---
title: "hashTable"
date: 2023-01-10
draft: false
category: [study]
subcategories: [Algorithm]
tags: [Algorithm]
---

`HashTable`은 데이터를 저장하는 자료구조 중 하나이다. 빠르게 데이터를 검색할 수 있는 자료구조 중 하나이다.

<!--more-->

해시테이블은 `O(1)`의 시간복잡도를 가지게 된다. 그 이유는 버킷(배열)을 사용하여 데이터를 저장하기 때문이다. 해시 테이블은 Key값을 해시함수에 집어넣어 계산된 index에 저장되는 형태이다.

해시테이블은 값의 충돌이 일어나기도 한다. 해시함수에서 구한 index값이 동일하면 충돌이 일어나는데, 이에 대한 처리로 `체이닝`, `선형조사`, `이차원조사`, `더블해싱` 등의 방법을 통해 충돌을 처리하게 된다.

1. 체이닝

체이닝에서는 같은 index로 해싱되는 원소를 모두 하나의 연결 리스트에 매달아서 관리한다. 해시 테이블의 크기가 m이면 최대 m개의 연결 리스트가 존재할 수 있다. 크기가 13인 해시 테이블에 원소들이 55, 13, 42, 70, 43, 44, 3, 94, 74, 74, 39, 86, 76, 40순으로 저장된다고 했을때의 그림을 살펴보도록 하겠다.

![](/images/study/Algorithms/hashTable/1.JPG)

기본적으로 해시테이블의 해시함수는 나누기 방법과 곱하기 방법이 있다. 나누기 방법은 `h(x) = x mod m (x : Key값 / m : 해시테이블의 크기)`의 해시함수를 이용한다. 해시함수를 이용해 구한 주소의 값이 충돌이 일어나게된다면, 연결리스트를 이용해 연결하게된다. 0~12배열안에 각각의 리스트가 하나씩 연결되어있는 구조이다. 가장 최근 충돌이 일어난 Key값이 앞에 연결되도록 해주는것이 가장 효율적이다.

```c
void hashChainAdd(element item, struct list* ht[]) {
    int hash_value = hashFunction(item.key, TABLE_SIZE);
    list* ptr;
    list* node_before = NULL, * node = ht[hash_value];
    for (; node; node_before = node, node = node->link) {
        if (node->item.key == item.key) {
            fprintf(stderr, "이미 탐색키가 저장되어 있음\n");
            return;
        }
    }
    ptr = (list*)malloc(sizeof(list));
    ptr->item = item;
    ptr->link = NULL;
    if (node_before)
        node_before->link = ptr;
    else
        ht[hash_value] = ptr;
}
```

2. 선형조사

선형조사는 개방주소 방법으로 체이닝과 다르게 추가공간을 허용하지 않는다. 충돌이 일어나도 어떻게든 주어진 테이블 공간에서 해결하도록 한다. 선형조사 방법의 해시함수는 `h(x) = (h(x) + i) mod m (i : 충돌이 일어난 횟수 / x : Key값 / m : 해시테이블의 크기)` 이렇게 정의된다. 선형조사법은 충돌이 일어나면 계속해서 다음 주소값으로 넘어가는 방법이기때문에 1차 군집이 발생한다는것이 취약점이다. 25, 13, 16, 15, 7, 28, 31, 20, 1, 38 순으로 저장된다 했을때 그림을 살펴보도록 하겠다.

![](/images/study/Algorithms/hashTable/2.JPG)

이렇게 충돌이 일어나면 다음 주솟값으로 넘어간다는 특징때문에 선형조사법은 파란부분과 동일하게 1차 군집에 취약하다.

```c
int hashFunction(int key, int size) {
    return key % size;
}

void hashLpAdd(int item, int ht[], int size) {
    int i, hash_value, cnt = 1;
    hash_value = i = hashFunction(item, size);
    printf("hash_address=%d", i);
    while (!(ht[i] == 0)) {
        i = hashFunction(item + cnt, size);
        cnt++;
        printf("->%d", i);
    }
    ht[i] = item;
    printf("\n");
}
```

3. 이차원조사

이차원조사의 해시함수는  `h(x) = (h(x) + i^2) mod m (i : 충돌이 일어난 횟수 / x : Key값 / m : 해시테이블의 크기)`로 사용이 가능하다. 이차원조사법은 충돌이 일어나면 i의 제곱수만큼 주솟값을 이동시키는 방법이다. 이차원조사법은 선형조사법과 다르게 2차군집이 발생한다는것이 취약점이다. 25, 13, 16, 15, 7, 28, 31, 20, 1, 38 순으로 저장된다 했을때 그림을 살펴보도록 하겠다.

![](/images/study/Algorithms/hashTable/3.JPG)

파란부분과 동일하게 2차 군집에 취약한 것을 확인 할 수 있다.

```c
int hashFunction(int key, int size) {
    return key % size;
}

void hashQpAdd(int item, int ht[], int size) {
    int i, hash_value, cnt = 1;
    hash_value = i = hashFunction(item, size);
    printf("hash_address=%d", i);
    while (!(ht[i] == 0)) {
        i = hashFunction(hash_value + cnt * cnt, size);
        cnt++;
        printf("->%d", i);
    }
    ht[i] = item;
    printf("\n");
}
```

4. 더블해싱

더블해싱은 말 그대로 두개의 함수를 이용하는 충돌 해결 방법이다. 더블해싱의 해시함수는 `h(x) = (h(x) + if(x)) mod m (i : 충돌이 일어난 횟수 / x : Key값 / m : 해시테이블의 크기)`로 사용이 가능하다.
h(x)와 f(x)는 서로 다른 해시함수로, 충돌이 생겨 다음에 볼 주소를 계산할 때 두 번째 해시 함수 값만큼 이동하게된다. 15, 28, 41, 67, 19순으로 저장했을때 해시테이블을 살펴보겠다.

![](/images/study/Algorithms/hashTable/4.JPG)

15, 28, 41, 67이 모두 주소 2로 해싱이 되지만 두번째 f(x)의 함수값(f(28) = 7, f(41) = 9, f(67) = 2)이 모두 달라 주소가 모두 달라진다.

```c
int hashFunction(int key, int size) {
    return key % size;
}

int hashFunction2(int key) {
    return (1 + (key % 11));
}

void hashDhAdd(int item, int ht[], int size, int num1) {
    int i, hash_value, cnt;
    hash_value = i = hashFunction(item, size);
    cnt = hashFunction2(item);
    cnt1 = 0;
    printf("hash_address=%d", i);
    while (!(ht[i] == 0)) {
        i = hashFunction(hash_value + cnt * cnt1, size);
        printf("->%d", i);
        cnt1++;
    }
    ht[i] = item;
    printf("\n");
}
```