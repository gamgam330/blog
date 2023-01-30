---
title: "topologicalSort"
date: 2023-01-26
draft: false
category: [study]
subcategories: [Algorithm]
tags: [Algorithm]
---

`위상정렬`은 순서가 정해져 있는 일련의 작업을 차례때로 수행해야 할 때 사용할 수 있는 알고리즘이다.

<!--more-->

완수해야할 여러 가지 일이 있고 이들 간에 상호 선후 관계가 있다. 한 사람이 라면을 끓일 떄는 어떤 순서로 일을 할 수 있는지 살펴보도록 하겠다.

1. 냄비에 물 붓기
2. 점화
3. 라면봉지 뜯기
4. 라면 넣기
5. 스프 넣기
6. 계란 풀어넣기

이러한 방법 이외에도

1. 라면봉지 뜯기
2. 냄비에 물 붓기
3. 점화
4. 라면 넣기
5. 스프 넣기
6. 계란 풀어넣기

의 방법으로 진행되어도 무방하다.

이렇게 라면봉지뜯기, 냄비에 물 붓기에는 선후 관계가 없다. 라면 넣기와 스프 넣기도 선후관계가 없으므로 순서가 뒤바뀌어도 무방하다. 

![](/images/study/Algorithms/topologicalSort/1.JPG)

위의 그림에서 작업 i와 작업 j 사이에 간선 (i,j)가 존재한다면 작업 i는 반드시 작업 j보다 먼저 수행된다. 그림의 모든 간선에 대해서 이 성질만 만족하면 어떤 순서라도 좋다. 이런 성질을 만족하는 정렬을 위상정렬이라고 한다.

위상정렬은 싸이클이 없는 유향 그래프에서 모든 정점을 일렬로 나열하되 정점 x에서 정점 y로 가는 간선이 있으면 x는 반드시 y보다 앞에 존재하게된다.

![](/images/study/Algorithms/topologicalSort/2.JPG)
![](/images/study/Algorithms/topologicalSort/3.JPG)
![](/images/study/Algorithms/topologicalSort/4.JPG)
![](/images/study/Algorithms/topologicalSort/5.JPG)

위 사진은 첫번째 위상정렬 방법이다. 이 첫번째 위상정렬은 각 정점에 연결된 간선을 구하고 연결된 간선이 없다면 출력 후 정점이 사라지는 방법이다. 정점이 사라지며 간선또한 사라지게된다. 이렇게 계속해서 자기 자신에게 연결된 간선이 0개인 정점을 출력해 위상정렬 할 수 있다.

```c
void topo_sort(GraphType1* g) {
    int i;
    StackType s;
    GraphNode* node;

    int* in_degree = (int*)malloc(g->n * sizeof(int));
    for (i = 0; i < g->n; i++) {
        in_degree[i] = 0;
    }
    for (i = 0; i < g->n; i++) {
        GraphNode* node = g->adj_list[i];
        while (node != NULL) {
            in_degree[node->vertex]++;
            node = node->link;
        }
    }

    init(&s);
    for (i = 0; i < g->n; i++) {
        if (in_degree[i] == 0) push(&s, i);
    }

    while (!is_empty(&s)) {
        int w;
        w = pop(&s);
        printf("정점 %d ->", w);
        node = g->adj_list[w];
        while (node != NULL) {
            int u = node->vertex;
            in_degree[u]--;
            if (in_degree[u] == 0) push(&s, u);
            node = node->link;
        }
    }

    free(in_degree);
    printf("\n");
}
```
![](/images/study/Algorithms/topologicalSort/6.JPG)
![](/images/study/Algorithms/topologicalSort/7.JPG)
![](/images/study/Algorithms/topologicalSort/8.JPG)
![](/images/study/Algorithms/topologicalSort/9.JPG)
![](/images/study/Algorithms/topologicalSort/10.JPG)

이 그림은 두번째 위상정렬 방법이다. 이 방법은 깊이우선탐색을 이용한 방법으로, 더 이상 간선이 연결되어 있지 않을때 까지 깊이우선탐색을 진행하게 된다. 위 그림은 스프넣기를 시작으로 깊이우선탐색을 하면 계란 풀어넣기에서 더이상 갈 곳이 없게된다. 이렇게 계란 풀어넣기를 연결리스트 맨앞에 연결하고, 다시 되돌아 나와 스프넣기를 또다시 연결리스트 맨앞에 연결하게 된다. 그 후에 냄비에 물 붓기 부터 다시 시작해 점화 -> 라면넣기로 들어가게 되고 더이상 라면넣기에서 들어갈수있는 곳이 없기때문에 라면넣기를 다시 아까 만들었던 연결리스트 맨앞에 연결시켜준다. 이렇게 반복을 통해 위상정렬이 된 리스트를 구할 수 있다.

```c
void DFS_TS(GraphType1* g, int v) {
    GraphNode* w;
    GraphNode* newNode = malloc(sizeof(GraphNode));
    visited[v] = 1;
    for (w = g->adj_list[v]; w; w = w->link) {
        if (!visited[w->vertex])
            DFS_TS(g, w->vertex);
    }
    newNode->vertex = v;
    newNode->link = node;
    node = newNode;
}

void topologicalSort2(GraphType1* g) {
    int start;

    printf("\n\n시작할 정점 입력하세요 : ");
    scanf("%d", &start);

    for (int i = 0; i < g->n; i++) {
        visited[i] = 0;
    }

    DFS_TS(g, start);
    printList(g);
    for (int i = 0; i < g->n; i++) {
        if (!visited[i]) {
            printf("\n처음으로 돌아갑니다\n");
            DFS_TS(g, i);
            printList(g);
        }
    }
}
```