---
title: "AVLTree"
date: 2023-01-11
draft: false
category: [study]
subcategories: [Algorithm]
tags: [Algorithm]
---

`AVL트리` 모든 노드의 왼쪽과 오른쪽 서브트리의 높이차가 1이하인 이진탐색트리를 말한다.

<!--more-->

AVL트리의 경우 트리가 비균형 상태가 되면 스스로 노드들을 재배치 하게된다. AVL트리를 이용해 탐색과 저장은 최악, 최선, 평균 시간복잡도 모두 `O(logn)` 의 시간복잡도를 가지게 된다. 기본적인 이진탐색트리의 경우 균형이 깨져 한쪽으로만 치우치게 된다면, `O(n)`의 시간복잡도를 가지게 되지만, AVL트리는 스스로 노드를 재배치 하며 항상 균형이 맞는 트리를 가지게 된다.

전체적으로 밸런스를 유지하는데, 밸런스의 값이 -1 ~ 1사이를 벗어나면 균형이 깨지게 된다. 

```c
int get_height(treeNode* node) {
    int height = 0;
    if (node != NULL) {
        height = 1 + MAX(get_height(node->left), get_height(node->right));
    }
    return height;
}

int get_balance(treeNode* node) {
    if (node == NULL) return 0;
    return get_height(node->left) - get_height(node->right);
}
```

AVL트리는 균형을 맞추는데에 4가지 케이스가 존재한다 `RR 케이스`, `LL 케이스`, `LR 케이스`, `RL 케이스` 이렇게 4가지의 케이스가 존재한다.

1. RR 케이스

![](/images/study/Algorithms/AVLTree/1.JPG)

RR케이스는 위의 그림과 동일하게 오른쪽으로 기울어져 있는 트리를 균형이 맞도록 `Left Rotation`해주는 방법이다. 해당 노드를 기준으로 `좌회전`을 적용하면 불균형이 해소된다. 기본적으로 코드를 설명하자면, 해당 노드가 자기 자신의 자식노드의 왼쪽자식을 연결하고, 자식노드가 해당노드를 연결하는 형태로 회전이 일어난다.

2. LL 케이스

![](/images/study/Algorithms/AVLTree/2.JPG)

LL케이스는 위의 그림과 동일하게 왼쪽으로 기울어져 있는 트리를 균형이 맞도록 `Right Rotation`해주는 방법이다. 해당 노드를 기준으로 `우회전`을 적용하면 불균형이 해소된다. LL 케이스의 경우도 마찬가지로 해당노드가 자신의 자식노드의 오른쪽 자식을 연결하고 자식노드가 해당노드를 연결하는 형태로 회전이 일어난다.

3. LR 케이스

![](/images/study/Algorithms/AVLTree/3.JPG)

LR케이스는 위의 그림과 동일하게 지그재그로 기울어져 있는 형태를 띈다. 먼저 `Left Rotation`을 이용해 위의 설명처럼 `좌회전`이 발생하게 된다. 이후 LR 케이스는 LL 케이스의 형태로 모양이 바뀌게 되고, 여기서 `Right Rotation`을 통해 `우회전`을 한번 더 하고나면 불균형이 해소된다.

4. RL 케이스

![](/images/study/Algorithms/AVLTree/4.JPG)

RL케이스는 LR케이스와 반대모양의 불균형 트리이다. 때문에 LR트리와 반대로 회전이 일어나게 된다. 먼저 `Right Rotation`을 통해 `우회전`이 발생하면, RR 케이스의 형태로 모양이 바뀌게 되고, 여기서 `Left Rotation`을 통해 `좌회전`을 한번 더 하게되면 불균형이 해소된다.

```c
treeNode* rotate_right(treeNode* parent) {
    if (parent->left != NULL) {
        treeNode* child = parent->left;
        parent->left = child->right;
        child->right = parent;
        return child;
    }
    else {
        return;
    }
}

treeNode* rotate_left(treeNode* parent) {
    if (parent->right != NULL) {
        treeNode* child = parent->right;
        parent->right = child->left;
        child->left = parent;
        return child;
    }
    else {
        return;
    }

}

treeNode* insert(treeNode* node, int key) {
    if (node == NULL) {
        return create_node(key);
    }
    if (key < node->data) node->left = insert(node->left, key);
    else if (key > node->data) node->right = insert(node->right, key);
    else return node;

    int balance = get_balance(node);

    if (balance > 1 && key < node->left->data) return rotate_right(node);
    if (balance < -1 && key > node->right->data) return rotate_left(node)

    if (balance > 1 && key > node->left->data) {
        node->left = rotate_left(node->left); 
        return rotate_right(node);
    }

    if (balance < -1 && key < node->right->data) { 
        node->right = rotate_right(node->right);
        return rotate_left(node);
    }
    return node;
}


treeNode* min_value_node(treeNode* node) {
    treeNode* current = node;
    while (current->left != NULL)
        current = current->left;
    return current;
}

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

    int balance = get_balance(root);
    if (balance > 1 && get_balance(root->left) >= 0) {
        return rotate_right(root);
    }
    if (balance > 1 && get_balance(root->left) < 0) {
        root->left = rotate_left(root->left);
        return rotate_right(root);
    }
    if (balance < -1 && get_balance(root->right) <= 0) {
        return rotate_left(root);
    }
    if (balance < -1 && get_balance(root->right) > 0) {
        root->right = rotate_right(root->right);
        return rotate_left(root);
    }
    return root;
}
```