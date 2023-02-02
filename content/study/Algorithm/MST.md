---
title: "Minimum Spanning Tree"
date: 2023-02-02
draft: false
category: [study]
subcategories: [Algorithm]
tags: [Algorithm]
---

`MST(최소 신장트리)`는 간선들의 가중치 합이 최소가 되는 신장트리이다.

<!--more-->

각 정점에 이어진 간선의 비용을 가중치라고 했을때, 최소 신장 트리는 사이클 없이 모든 정점을 연결했을대 가중치합이 최소가 되는 트리를 말한다.

최소신장트리를 구하기 위한 알고리즘으로 `크루스칼 알고리즘`과 `프림 알고리즘`이 존재한다. 이 두 알고리즘은 `그리디 알고리즘`의 최적해를 보장하는 알고리즘들이다.

1. 크루스칼 알고리즘
   
n개의 정점으로 트리를 만드는 데는 n-1개의 간선이 필요하므로, 알고리즘은 최초에 간선이 하나도 없는 상태에서 시작하여 n-1개의 간선을 더하면 끝난다. 크루스칼 알고리즘은 프림 알고리즘처럼 하나의 트리를 키워나가는 방식이 아니고, 임의의 시점에 최소 비용의 간선을 더하므로 여러 개의 트리가 산재하게 된다. 

크루스칼 알고리즘을 사용하기위해 먼저 간선들의 가중치를 오름차순으로 정렬해야한다. 그 다음 가중치가 작은 간선부터 사이클을 형성하는지 검사해야한다. 여기서 사이클을 찾는 알고리즘은 `유니온-파인드`라는 알고리즘이 쓰이게 된다.

```java
public static int find(int a) {
		if(a == parent[a]) return a;
		return parent[a] = find(parent[a]);
}
	
public static void union(int a, int b) {
		a = find(a);
		b = find(b);
		if(a != b) parent[b] = a;
}
```

`유니온-파인드` 알고리즘은 같은 집합에 포함이 되어있는지 아닌지를 확인하는 알고리즘이다.

![](/images/study/Algorithms/MST/1.JPG)
![](/images/study/Algorithms/MST/2.JPG)
![](/images/study/Algorithms/MST/3.JPG)

모두 다 다른 집합에 존재하는 각각의 정점들이 자기 자신을 가리키도록 만든다. 만약 ```union(1, 2)```와 ```union(3,4)```함수가 실행된다면 두 번째 그림처럼 둘은 같은 집합이 아니였기때문에 같은집합으로 합쳐지게 된다. 이때 주의할점은 더 작은 값쪽으로 합쳐진다는 것이다.

만약 ```union(1,2)```가 실행되고 ```union(2,3)```함수가 실행된다면 3번째 사진과 동일하게 1,2,3은 같은 집합이 된다. 3번 정점은 2번 정점에 연결되었지만, 부모는 1번이다. 그 이유는 find함수의 재귀적 성질 때문이다. 재귀적 성질을 이용해 3번은 2번 정점을 연결하고 그 후에 2번 정점이 연결되어 있는 부모를 한번더 찾아내게 된다. 이렇게 2번 정점이 1번을 부모로 가지고 있기때문에 3번 정점은 2번 정점을 연결했지만, 재귀적 성질을 통해 2번 정점의 부모인 1번 정점을 부모로 가지게 된다.

이 그림과 동일하게 만약 1,2 / 2,3 / 3,1 정점들이 순서대로 union이 일어나게 되면, 사이클이 생기게 될것이다. 하지만 1,2 / 2,3 정점이 union되면서 1,2,3번 정점은 모두 부모를 1로 가지게 되었고, 3,1이 union이 일어나게 되면 이미 같은 부모를 가지고 있기때문에 union이 일어나지 않게 설정하면 사이클이 생기지 않는다.

![](/images/study/Algorithms/MST/4.jpg)
![](/images/study/Algorithms/MST/5.jpg)
![](/images/study/Algorithms/MST/6.jpg)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;
import java.util.StringTokenizer;

public class Main {
	static int[] parent;
	static ArrayList<Node> list;
	public static void main(String[] args) throws IOException{
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		int N = Integer.parseInt(st.nextToken());
		int M = Integer.parseInt(st.nextToken());
		list = new ArrayList<>();
		parent = new int[N+1];
		int temp = 0, cost = 0;
		
		for(int i = 0; i < M; i++) {
			st = new StringTokenizer(br.readLine());
			int start = Integer.parseInt(st.nextToken());
			int end = Integer.parseInt(st.nextToken());
			int weight = Integer.parseInt(st.nextToken());
			list.add(new Node(start, end, weight));
		}
		
		for(int i = 1; i <= N; i++) {
			parent[i] = i;
		}
		
		Collections.sort(list);
		for(int i = 0; i < list.size(); i++) {
			Node tmp = list.get(i);
			if(find(tmp.start) != find(tmp.end)) {
				temp += tmp.weight;
				union(tmp.start, tmp.end);
				cost = tmp.weight;
			}
		}
		System.out.print(temp - cost);
	}
	
	public static int find(int a) {
		if(a == parent[a]) return a;
		return parent[a] = find(parent[a]);
	}
	
	public static void union(int a, int b) {
		a = find(a);
		b = find(b);
		if(a != b) parent[b] = a;
	}
	
	static class Node implements Comparable<Node>{
		int start;
		int end;
		int weight;
		
		Node(int start, int end, int weight){
			this.start = start;
			this.end = end;
			this.weight = weight;
		}
		@Override
		public int compareTo(Node o) {
			return weight - o.weight;
		}
	}
}
```

C언어로 작성한 코드가 사라져 백준 1647 크루스칼 문제를 대체해서 올려놓도록 하겠다..........

2. 프림 알고리즘

프림 알고리즘은 집합 S를 공집합에서 시작하여 모든 정점을 포함할 때까지 키워나간다. 맨 처음 정점을 제외하고는 정점을 하나 더할 때마다 간선이 하나씩 확정된다. 크루스칼알고리즘과 다르게 현재 연결된 정점들의 간선들 중 가중치가 가장 작은 간선을 선택해 뻗어나아가는 형태이다. 먼저 그림을 통해 살펴보겠다

![](/images/study/Algorithms/MST/7.jpg)
![](/images/study/Algorithms/MST/8.jpg)
![](/images/study/Algorithms/MST/9.jpg)

이렇게 현재 연결된 정점들의 간선들 중 가중치가 가장 작은 간선을 채택해 뻗어나가게 된다. 

```c
#define INF 999
int extractMin(int n, int* visited, int* dist) {
    int v = -1;
    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            v = i;
            break;
        }
    }
    for (int i = 0; i < n; i++) {
        if (!visited[i] && (dist[i] < dist[v]))                                    //가중치가 제일 작은 간선 찾기.
            v = i;
    }
    return v;
}

int primMat(GraphType1* g, int s, int n, int* visited, int* dist) {
    int i, v, sum = 0;
    for (int u = 0; u < n; u++) {
        dist[u] = INF;
        visited[u] = 0;
    }
    dist[s] = 0;
    for (int i = 0; i < n; i++) {
        int u = extractMin(n, visited, dist);
        visited[u] = 1;

        if (dist[u] == INF) {
            printf("\n연결이 되어있지 않습니다.");                                 //간선 연결 안됐으면 종료
            return;
        }
        printf("==========정점 [%d] 추가 ==========\n", u);
        printf("\ndist");
        for (int j = 0; j < n; j++) {
            printf("   [%d]", j);
        }
        printf("\n\n");

        printf("select  ");
        for (int j = 0; j < n; j++) {
            if (visited[j]) printf("O     ");
            else printf("X     ");
        }
        printf("\n\n   ");
        for (int j = 0; j < n; j++) {
            if (dist[j] != INF) printf("%6d", dist[j]);
            else printf("   INF");
        }
        printf("\n");
        for (v = 0; v < n; v++) {
            if (g->weight[u][v] != INF) {                                           //현재 보고있는 정점의 간선을 찾는 부분
                if (!visited[v] && g->weight[u][v] < dist[v]) {                     //그 간선을 연결하지 않았고, 간선의 가중치보다
                    dist[v] = g->weight[u][v];                                      //현재 저장된 dist가 크면 dist 수정
                }
                printf("\n");
            }
        }
        }
    printf("\n최종 dist값 >>>\n");
    for (i = 0; i < n; i++) {
        printf("%5d", dist[i]);
        sum += dist[i];
    }
    return sum;
}
```

