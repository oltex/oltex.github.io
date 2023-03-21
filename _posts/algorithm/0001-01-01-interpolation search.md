---
title: "보간 탐색(Interpolation Search)"
categories:
  - algorithm
tags:
  - tag
---
> ## 개요

탐색 알고리즘 중 하나인 보간 탐색입니다.<br>
이진 탐색을 개신시킨 알고리즘입니다.<br>
<br>
이진 탐색의 과정을 그대로 진행하지만<br>
탐색할 원소의 위치를 예측을 통해 좀더 근접하게 접근합니다.
> ## 구조

이진 탐색과 기본적인 방법은 똑같습니다.<br>
허나 이진 탐색은 찾을 원소가 어떠한 값을 가지고 있더라고 중앙에서<br>
부터 시작한다는 단점이 존재합니다.<br>
<br>
보간 탐색은 이러한 탐색 위치를 예측하여 가깝게 만듭니다.<br>
예측에 사용되는 방법은 다음과 같습니다.
```
int mid = static_cast<float>(find - arr[first]) / (arr[last] - arr[first]) * (last - first) + first;
```
이를 정리해보면<br>
찾으려고 하는 대상과 배열의 원소중 가장 작은 값을 빼고<br>
배열에서 가장 큰 값으로 나눠서 퍼센트를 구합니다.<br>
<br>
이후 이를 배열의 인덱스의 갯수와 곱하여 해당하는 인덱스의 거리를 알아냅니다.<br>
마지막으로 fisrt인덱스를 더하여 최종적인 인덱스를 찾습니다.<br>
<br>
조금더 쉽게 말로 정리하자면<br>
찾으려하는 값을 배열의 가장 작은 원소와 큰 원소와 비교하여 (0~1)의 값으로 변화시키고<br>
이를 배열의 인덱스 갯수와 곱하여 해당하는 인덱스를 알아내는 작업입니다.<br>
<br>
이러한 원리에 의하여 배열의 값과 배열의 인덱스가 선형적인 구조로 이루어져 있다면<br>
단번에 원소를 찾는 경우도 존재하게 됩니다.
> ## 구현

보간 탐색을 구현해 보겠습니다.<br>
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
```cpp
void main(void) {
	int arr[9] = { 1, 2, 3, 7, 9, 12, 21, 23, 27 };
	std::cout << Interpolation_Search(arr, 9, 7) << std::endl;
};
```
기본적으로 이진 탐색의 함수와 유사합니다.<br>
mid를 찾는 수식이 변경되었다는 것을 알 수 있습니다.<br>
딱 한가지 추가로 달라진 부분이 존재하는데 while문에 들어가는 조건식입니다.<br>
<br>
이진 탐색은 first <= last의 형태를 띄었지만<br>
보간 탐색은 arr[first] > find || arr[last] < find의 형태를 띄고있습니다.<br>
<br>
이러한 이유는 이진 탐색처럼 탐색할 인덱스를 중앙에 위치시키는 방식이<br>
아니기 때문에 배열에 없는 값을 탐색할 경우<br>
배열의 범위를 벗어날 가능성이 존재하기 때문입니다.<br>
<br>
예를 들어서 다음과 같은 상황에서,
```
1 2 3 5 6 7 8 9
1 2 (3) 5 6 7 8 9
1 2 3 [5 6 7 8 9]
```
여기까지는 이진 탐색처럼 정상이였지만<br>
이후 공식에 대입해 mid를찾게 되면
```
1 2 (3) [5 6 7 8 9]
```
또 3이 나오게 됩니다.(에러)
```
1 2 3 [5 6 7 8 9]
1 2 (3) [5 6 7 8 9]
1 2 3 [5 6 7 8 9]
1 2 (3) [5 6 7 8 9]
...
```
이후 부터는 범위를 벗어난 값이기 때문에 first가 잘못 설정되어<br>
무한 루프에 빠지게 됩니다.<br>
<br>
이러한 상황을 없에기 위해 인덱스를 통해 탈출문을 제어하는 것이 아닌<br>
원소의 값을 비교하여 탈출문을 제어하고 있습니다.
