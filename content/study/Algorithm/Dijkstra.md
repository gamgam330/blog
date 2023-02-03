---
title: "Dijkstra"
date: 2023-02-03
draft: false
category: [study]
subcategories: [Algorithm]
tags: [Algorithm]
---

`Dijkstra(다익스트라)` 알고리즘은 최단경로를 구하는 알고리즘으로 시작정점과 도착정점의 최단 경로를 구하는 알고리즘이다.

<!--more-->

최단 경로문제는 만약 음의 가중치가 존재하지 않으면 다익스트라 알고리즘을 사용하지만, 음의 가중치가 존재하면 `벨만-포드` 알고리즘을 사용한다. 하지만 나는 다익스트라 알고리즘에 대해서만 포스팅 하도록 하겠다.

다익스트라 알고리즘은 이전 포스팅에서 볼 수 있는 프림알고리즘의 원리와 비슷하다. 다익스트라 알고리즘은 정점 r에서 정점 v에 이르는 최단 거리를 위해 사용되고 프림알고리즘은 정점이 n개일때 n-1개의 간선을 모두 연결할때 가중치의 합이 가장 작은 트리를 구하는 차이가 있다.

![](/images/study/Algorithms/Dijkstra/1.jpg)
![](/images/study/Algorithms/Dijkstra/2.jpg)

이전 포스팅에서는 각 정점을 방문할 수 있는 `가장 작은 가중치값`을 저장했다면, 다익스트라 알고리즘에서는 각 정점에 방문할때 사용해야하는 `가장 작은 가중치의합`이 저장된다.

위의 그림처럼 시작정점을 기준으로 각 정점을 방문하는데 드는 가중치합을 각 정점에 저장해줌으로써, 1번정점에서 2번정점 / 1번정점에서 5번정점 등의 값을 구하고자 할때 그 정점에 저장된 가중치의 합들만 불러주면 된다.

```c
void Dijkstra_shortestPath(GraphType2* g, int start) {
    int u, w;

    for (int i = 0; i < g->n; i++) {
        distance[i] = g->weight[start][i];                      //가중치와 방문 여부 초기화
        visited2[i] = 0;
        prev[i] = g->PREV[start][i];
    }

    visited2[start] = 1;
    distance[start] = 0;                                        //시작점 초기화

    for (int i = 0; i < g->n - 1; i++) {
        u = nextVertex(g->n);
        visited2[u] = 1;


        for (w = 0; w < g->n; w++)
            if (!visited2[w]) {
                if (distance[u] + g->weight[u][w] < distance[w]) {          //지나쳐온 정점의 가중치와 현재 간선의 가중치의 합이 현재 저장되어있는 distance보다 작으면
                    distance[w] = distance[u] + g->weight[u][w];            //대입
                    prev[w] = u;                                            //지나온 간선 초기화
                }
            }
    }
}
```