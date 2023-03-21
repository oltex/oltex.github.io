---
title: "보간 탐색(Interpolation Search)"
categories:
  - algorithm
tags:
  - tag
---
> ## 개요

탐색 알고리즘 중 하나인 보간 탐색입니다.<br>
이진 탐색을 개신시킨 알고리즘입니다.

이진 탐색의 과정을 그대로 진행하지만
탐색할 원소의 위치를 예측을 통해 좀더 근접하게 접근합니다.

> ## 구조

이진 탐색과 기본적인 방법은 똑같습니다.
허나 이진 탐색은 찾을 원소가 어떠한 값을 가지고 있더라고 중앙에서
부터 시작한다는 단점이 존재합니다.

보간 탐색은 이러한 탐색 위치를 예측하여 가깝게 만듭니다.
예측에 사용되는 방법은 다음과 같습니다. 
```
int mid = static_cast<float>(find - arr[first]) / (arr[last] - arr[first]) * (last - first) + first;
```
이를 정리해보면
찾으려고 하는 대상과 배열의 원소중 가장 작은 값을 빼고
배열에서 가장 큰 값으로 나눠서 퍼센트를 구합니다.

이후 이를 배열의 인덱스의 갯수와 곱하여 해당하는 인덱스의 거리를 알아냅니다.
마지막으로 fisrt인덱스를 더하여 최종적인 인덱스를 찾습니다.

조금더 쉽게 말로 정리하자면
찾으려하는 값을 배열의 가장 작은 원소와 큰 원소와 비교하여 (0~1)의 값으로 변화시키고
이를 배열의 인덱스 갯수와 곱하여 해당하는 인덱스를 알아내는 작업입니다.

> ## 구현

보간 탐색을 구현해 보겠습니다.
보간 탐색을 진행하여 해당하는 인덱스를 반환하는 함수입니다.
```cpp
int Interpolation_Search(int arr[], int len, int find) {
	int first = 0;
	int last = len - 1;

	while (arr[first] > find || arr[last] < find) {
		int mid = static_cast<float>(find - arr[first]) / (arr[last] - arr[first]) * (last - first) + first;
		if (find == arr[mid])
			return mid;

		if (find < arr[mid])
			last = mid - 1;
		else
			first = mid + 1;
	}
	return -1;
}
```
