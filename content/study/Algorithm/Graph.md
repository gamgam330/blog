---
title: "Graph"
date: 2023-01-14
draft: false
category: [study]
subcategories: [Algorithm]
tags: [Algorithm]
---

그래프는 현상이나 사물을 정점과 간선으로 표현하는것으로, `점정(Vertex)`은 대상이나 개체를 나타내고 `간선(Edge)`은 이들 간의 관계를 나타낸다.

<!--more-->

먼저 그래프의 표현을 살펴보도록 하겠다. 그래프의 표현에는 두가지 방법을 살펴보도록 하겠다. `인접 행렬 방식`과 `인접 리스트 방식`이 존재한다. 

1. 인접행렬

그래프 G = (V, E)에서 정점의 총수가 N이라고 하자, 우선 N x N 행렬을 준비한다. 정점 i와 j간에 간선이 있으면 행렬의 (i,j) 원소와 (j,i)원소의 값을 1로 할당한다. 간선으로 연결된 두 정점은 `인접하다(Adhacent)`고 한다. 지금 설명한 그래프는 `무향 그래프`로 간선이 연결된 양쪽 정점에는 간선이 존재하는 형태이다. 하지만 `유향 그래프`의 경우 한쪽 방향으로만 연결이 될 수도 있다. 예를들어 i에서 j로는 간선이 있지만 j에서 i에는 간선이 없는 경우이다. 이럴경우에는 행렬의 (i,j)원소의 값을 1로 할당하지만 (j,i)원소의 값은 0으로 할당한다.

![](/images/study/Algorithms/Graph/1.JPG)
![](/images/study/Algorithms/Graph/2.JPG)

사진과 동일하게 `무향 그래프`와 `유향 그래프`가 그려진다.

![](/images/study/Algorithms/Graph/3.JPG)

위 사진과 동일하게 가중치가 존재할수도있다. 가중치란 간선을따라 이동하는데 필요한 비용이라고 생각하면 편하다. 가중치가 존재할때는, 인접행렬에 0과 1이 아닌 0과 가중치로 표현하면 된다. 만약 행렬의 값이 0이라면 간선이 없는것이고, 행렬의값이 0이 아닌 자연수라면 그만큼의 가중치가 필요한 간선이 존재한다는 의미이다.

행렬 표현법은 이해하기 쉽고 간선의 존재 여부를 즉각 알 수 있다는 장점이 있다. 정점 i와 정점 j의 인접 여부는 행렬의 (i,j)원소나 (j,i)원소의 값만 보면 알 수 있다. 대신 인접 행렬 방식은 N x N 행렬이 필요하므로 N^2에 비례하는 공간이 필요하고, 행렬의 준비 과정에서 행렬의 모든 원소를 채우는 데만 `N^2에 비례하는 시간`이 든다. 그러므로 O(N^2)미만의 시간이 소요되는 알고리즘이 필요한 경우에 행렬 표현법은 적절하지 않다.

2. 인접리스트

인접 리스트 표현법은 각 정점에 인접한 정점들을 리스트로 표현하는 방법이다. 각 정점마다 리스트를 하나씩 만든다. 여기에 각 정점에 인접한 정점들을 연결 리스트로 매단다. 인접 리스트 표현에서는 행렬 표현과 달리 존재하지 않는 간선은 표현상에 나타나지 않는다. 
인접리스트는 공간이 간선의 총수에 비례하는 양만큼 필요하므로 대체로 행렬 표현에 비해 공간의 낭비가 없다. 모든 가능한 정점 쌍에 비해 간선의 수가 적을 떄 특히 유용하다. 그렇지만 거의 모든 정점 쌍에 대한 간선이 존재하는 경우에는 오히려 리스트를 만드는 데 필요한 오버헤드만 더 든다.

![](/images/study/Algorithms/Graph/4.JPG)
![](/images/study/Algorithms/Graph/5.JPG)

무향 그래프의 경우 하나의 간선에 두개의 노드가 생성되어 연결된다. 인접 행렬과 다르게 직접 연결된 정점들을 노드로 만들어 인접리트스에 연결 시켜야 하기 때문이다.

![](/images/study/Algorithms/Graph/6.JPG)
![](/images/study/Algorithms/Graph/7.JPG)

사진과 동일하게 가중치가 존재하는 그래프는 가중치를 저장할 공간을 만들어 가 정점의 번호 / 가중치 를 저장시켜 연결시킨다.

![](/images/study/Algorithms/Graph/8.JPG)

이 사진은 `너비우선탐색`과 `깊이우선탐색`을 그린 사진이다. 일반적으로 그래프를 대상으로 하는 두 탐색에 대한 설명에 앞서 직관적인 이해를 돕기 위해 트리를 대상으로 그림을 그려본 것이다. 

`너비우선탐색(BFS, Breadth-First Search)`는 루트 노드에서 시작해서 인접한 노드를 먼저 탐색하는 방법이다. 즉, 먼저 루트의 자식을 차례로 방문한다. 다음으로 루트 자식의 자식을 방문하는식으로 방문한다 말그대로 정말 양옆으로 먼저 탐색하는 방법이다. 너비우선탐색은 `큐(Queue)`를 사용한다.

`깊이우선탐색(DFS, Deapth-First Search)`는 자식 정점을 하나 방문한 다음 아래로 내려갈 수 있는 곳까지 내려간다. 더 이상 내려갈 수가 없으면 위로 되돌아오다가 내려갈 곳이 생기만 즉각 내려간다. 깊이우선탐색은 `재귀`와 `스택(Stack)`을 사용한다.

![](/images/study/Algorithms/Graph/9.JPG)
![](/images/study/Algorithms/Graph/10.JPG)

그림에서 검은색의 굵은 간선들을 각 정점을 처음으로 방문할 때 사용한 간선들이다. 그래프 G에서 이 간선들만 남기면 트리가 되는데 이 트리를 너비 우선 트리라 한다.

![](/images/study/Algorithms/Graph/11.JPG)
![](/images/study/Algorithms/Graph/12.JPG)
![](/images/study/Algorithms/Graph/13.JPG)

그림에서 검은색 굵은 간선들의 각 정점을 처음으로 방문할 때 사용된 간선들이다. 각 정점을 처음으로 방문할 때 사용한 간선들로 만들어진 트리이다.

```c
int bfsMat(GraphType1* g, int v, int* visited) {        //인접행렬
    QueueType q;
    queueInit(&q);
    visited[v] = 1;
    printf("[정점 %d ->] ", v);
    enqueue(&q, v);
    while (!isEmpty(&q)) {
        v = dequeue(&q);
        for (int w = 0; w < g->node; w++) {
            if (g->adjMat[v][w] && !visited[w]) {
                visited[w] = 1;
                printf("[정점 %d ->] ", w);
                enqueue(&q, w);
            }
        }
    }
    return v;
}

void bfsList(GraphType2* g, int v) {                    //인접리스트
    GraphNode* w;
    QueueType q;
    queueInit(&q);
    visited2[v] = 1;
    printf("정점 %d -> ", v);
    enqueue(&q, v);
    while (!isEmpty(&q)) {
        v = dequeue(&q);
        for (w = g->adjList[v]; w; w = w->link) {
            if (!visited2[w->vertex]) {
                visited2[w->vertex] = 1;
                printf("정점 %d -> ", w->vertex);
                enqueue(&q, w->vertex);
            }
        }
    }
}
```

너비우선탐색에 대한 코드이다.

```c
void dfsMat(GraphType1* g, int v) {             //인접행렬
    visited[v] = 1;
    printf("[정점 %d ->] ", v);
    for (int w = 0; w < g->node; w++) {
        if (g->adjMat[v][w] && !visited[w])
            dfsMat(g, w);
    }
}

void dfsList(GraphType2* g, int v) {           //인접리스트
    GraphNode* w;
    visited[v] = 1;
    printf("[정점 %d ->] ", v);
    for (w = g->adjList[v]; w; w = w->link) {
        if (!visited[w->vertex])
            dfsList(g, w->vertex);
    }
}
```

깊이우선탐색에 대한 코드이다.