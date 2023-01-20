---
title: "Binary Search Tree"
date: 2023-01-16
draft: false
category: [study]
subcategories: [Algorithm]
tags: [Algorithm]
---

`이진검색트리(Binary Search Tree)`는 검색 방법 중 한가지로, 다음과 같은 특징을 갖는다.

<!--more-->

1. 이진 검색 트리의 각 노드는 키 값을 하나씩 갖는다. 각 노드의 키 값은 모두 달라야 한다.
2. 최상위 레벨에 루트 노드가 있고, 각 노드는 최대 두 개의 자식 노드를 갖는다.
3. 임의의 노드의 키 값은 자신의 왼쪽에 있는 모든 노드의 키 값보다 크고, 오른쪽에 있는 모든 노드의 키 값보다 작다.

지금부터 이진검색트리의 검색, 삽입, 삭제에 대해 살펴보도록 하겠다.

`검색`에서 만약 키가 x인 노드를 검색하고자 할때 트리에 키가 x인 노드가 존재하면 해당 노드를 리턴하고, 존재하지 않으면 NIL을 리턴하도록 한다.

```c
treeNode* search_node(treeNode* node, int data) {
    if (node == NULL)
        return NULL;
    if (data == node->data)
        return node;
    else if (data < node->data)
        return search_node(node->left, data);
    else
        return search_node(node->right, data);
}
```

알고리즘의 입력으로는 검색할 트리의 루트 노드 node와 검색하고자 하는 키 data가 주어진다. 루트 노드 node의 키 값과 data를 비교해 data가 더 작으면 node의 왼쪽 서브 트리로 가서 data를 찾는다. data가 더 크면 node의 오른쪽 서브 트리로 가서 data를 찾는다. 만약 node가 NIL이면 검색 실패, 값을 찾는다면 검색 성공이다.

`삽입`에서는 원소 x를 이진검색트리에 삽입하기위해 우선 이진 검색트리에 x를 키 값으로 가진 노드가 없어야 한다. 원소 x를 삽입할 자리를 찾기 위해서는 우선 실패하는 검색을 한 번 수행해야 한다. 즉, 루트 노드에서 x에 대한 검색을 수행해 임의의 리프 노드에 이르러 더 이상 내려갈 곳이 없음이 확인되면 x를 그 리프 노드의 자식으로 매단다. 그림으로 30, 20, 25, 40, 10, 35의 순서로 원소가 삽입되는 경우를 살펴보도록 하겠다.

![](/images/study/Algorithms/BST/1.JPG)
![](/images/study/Algorithms/BST/2.JPG)

아무것도 없는 상태에서 30이 삽입되면 30 하나만으로 이루어진 트리가 만들어진다. 
20이 입력되면 30보다 작으므로 30의 왼쪽에 매단다.
25가 입력되면 30보다 작으므로 30의 왼쪽으로 간다. 20보다는 크므로 20의 오른쪽에 매단다.

이런식으로 작다면 왼쪽 크다면 오른쪽으로 내려가면서 NIL을 만났을때 매달면된다.

```c
treeNode* insert(treeNode* node, int data) {
    if (node == NULL) {
        return create_node(data);
    }
    if (data < node->data) node->left = insert(node->left, data);
    else if (data > node->data) node->right = insert(node->right, data);
    else return node;

    return node;
}
```

내가 짠 코드에서는 따로 중복 검사를 하지않았다.

이진검색트리에서 삽입은 노드가 삽입될 자리만 정해지면 간단하다. 하지만 `삭제`의 경우는 상제할 노드가 정해진 이후가 더 복잡하다. 이진 검색 트리에서 노드 r을 삭제하고자 할 때는 다음 세가지의 경우에 따라 각각 다르게 처리해 주어야한다.

r이 리프노드인 경우
r의 자식노드가 하나인 경우
r의 자식노드가 두 개인 경우

```c
treeNode* delete_node(treeNode* root, int data) {
    if (root == NULL)return root;

    if (data < root->data)
        root->left = delete_node(root->left, data);

    else if (data > root->data)
        root->right = delete_node(root->right, data);

    else {
        if (root->left == NULL) {
            treeNode* temp = root->right;
            free(root);
            return temp;
        }
        else if (root->right == NULL) {
            treeNode* temp = root->left;
            free(root);
            return temp;
        }
        treeNode* temp = min_value_node(root->right);
        root->data = temp->data;
        root->right = delete_node(root->right, temp->data);
    }
    return root;
}
```

리프노드이거나 자식이 하나인 경우에는 부모의 부모가 자식을 연결해주면 되지만, 자식이 두개인 경우에는 조금 복잡하다. 자식이 두개인 경우에는 왼쪽 서브트리에서 가장 큰 리프노드나 오른쪽 서브트리에서 가장 작은 리프노드와 부모노드의 자리를 바꾸어 연결을 끊어 줘야한다.

이렇게 이진검색트리의 `검색`, `삽입`, `삭제`에 대해 알아보았다. 이진검색트리는 데이터의 검색, 삽입, 삭제에서 쓰이는 알고리즘으로 최악의 경우에 대해 알아보겠다.

![](/images/study/Algorithms/BST/3.JPG)

이렇게 10, 20, 25, 30, 40의 순서로 삽입이 될 경우, 오른쪽으로 기울어진 트리가 완성된다 이런 트리는 40의 데이터를 찾고자 할때 5번의 비교가 이루어진다. 즉 시간복잡도가 `O(n)`이라는 의미이다. 이진검색트리가 균형이 잡히면 최악의 경우라도 검색시간은 `O(logn)`의 시간복잡도를 가진다.

```c
int sum = 0, count1 = 1;
int ipl(treeNode* node, int count) {
    if (node != NULL) {
        sum += count;
        ipl(node->left, count + 1);
        ipl(node->right, count + 1);
    }
    return sum;
}
```

이렇게 `IPL(이진검색트리의 평균 검색시간)`을 확인해 트리의 성능을 비교할 수 있다. 이렇게 균형이 깨지면 취약한 이진검색트리를 보완한 트리가 `레드블랙트리`와 `AVL트리`이다.