---
title: "quickSort"
date: 2023-01-03
draft: false
category: [study]
subcategories: [Algorithm]
tags: [Algorithm]
---

`병합정렬, 퀵정렬, 힙정렬`에 대해 정리해보도록하겠다.
단순정렬의 시간복잡도는 삽입정렬을 제외한 모든정렬이 O(N^2)이다. 하지만 지금부터 볼 병합정렬과 퀵정렬 힙정렬은 모두 시간복잡도가 단순정렬보다 작다.

<!--more-->

퀵정렬에 대해 살펴보겠다. 퀵정렬은 평균시간복잡도가 `O(nlogn)`으로 병합정렬, 힙정렬과 동일하다. 평균적인 경우에 병합정렬과 힙정렬보다 월등히 속도가 빨라 실무에서 가장 많이 쓰이는 정렬의 종류중 하나이다.

최악의 경우에는 `O(n^2)`의 시간복잡도를 갖는다. 지금부터 왜 이러한 시간복잡도를 갖게됐는지 알아보도록 하겠다.

퀵정렬은 선처리 작업이 존재한다. 병합정렬은 후처리식으로 병합과정이 있었지만, 퀵정렬은 선처리작업으로 분할이 존재한다. 점화식으로 표현하게 된다면 `2T(n/2) + Q(n) (선처리식)`으로 표현이 가능하다. 뒤에 붙은 `Q(n)`의 경우 병합정렬에선 후처리식에 해당했지만, 퀵정렬에선 선처리식에 해당한다.

퀵정렬에대해 그림으로 설명해보겠다.

![](/images/study/Algorithms/quickSort/1.jpg)
![](/images/study/Algorithms/quickSort/2.jpg)

퀵정렬은 i와 j라는 변수를 가지고 구역을 표현하게 된다. 사진과 동일하게 배열의 맨앞부터 하나하나 훑어가며 만약 피봇인 15보다 크다면 j구역만 늘어나게되고, 피봇인 15보다 작은값을 만나면 j구역의 맨처음값과 위치를 스왑하며 i구역도 늘어난다. 이렇게 확인과 스왑이 끝나게 된다면 피봇인 15는 i구역과 j구역 사이로 들어가게된다. 한단계가 끝났다면 이전의 피봇을 기준으로 왼쪽과 오른쪽 구역으로 나뉘게 되고, 왼쪽과 오른쪽구역은 각각 이와 같은 방법으로 진행되게된다. 이렇게 진행되면 매단계에서 피봇의 값은 자기자리를 찾아가게 된다. 사진에서도 15보다 작은값은 3개이고 15보다 큰값은 6개로 정확히 15가 4번쨰에 위치하게 된것을 볼 수 있다. 이렇게 위치를 잡은 피봇값은 다시는 움직이지 않는다.

```C
void qickSort1(int* arr, int left, int right, int len) {
	if (left < right) {
		int mid = partition1(arr, left, right, len);
		qickSort1(arr, left, mid - 1, len);
		qickSort1(arr, mid + 1, right, len);
	}
}

int partition1(int* arr, int left, int right, int len) {
	int x = arr[right];
	int i = left - 1;											
	printf("%d단계>>\n", cnt++);
	printf("[기준값 : %d]\n", x);
	for (int j = left; j < right; j++) {									
		if (arr[j] < x) {
			int tmp = arr[++i];
			arr[i] = arr[j];
			arr[j] = tmp;
		}
	}
	int tmp = arr[i + 1];
	arr[i + 1] = arr[right];
	arr[right] = tmp;							
	PrintArray(arr, left, right, len);
	return i + 1;											
}
```

퀵정렬이 최악의경우 `O(n^2)`의 시간복잡도를 갖는 이유는 피봇의 위치가 계속해서 맨뒤나 맨앞으로 잡히게 된다면, 정렬된 피봇을 기준으로 왼쪽과 오른쪽으로 분할되는것이 아니라 한쪽으로만 치우쳐 분할이 되게된다. 이럴경우에는 배열에 존재하는 모든 원소들을 비교해야 정렬이 끝이난다.