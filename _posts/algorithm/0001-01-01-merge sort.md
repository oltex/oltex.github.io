---
title: "합병 정렬(Merge Sort)"
categories:
  - algorithm
tags:
  - tag
---
> ## 개요

정렬 알고리즘 중 하나인 합병 정렬입니다.<br>
배열을 절반씩 분할하는 과정을 거치고 이를 합병하며 정렬하는 알고리즘입니다.<br>
<br>
합병 정렬은 분할 정복(divide and conquer)이라는<br>
알고리즘 디자인 기법에 근거하여 만들어진 정렬 방법입니다.
> ## 분할 정복

분할 정복이란 복잡한 문제를 복잡하지 않은 문제로 분할하여 정복하는 방법입니다.<br>
단 분할하였으니 다시 결합하는 과정을 거쳐야합니다.<br>
<br>
즉 다음 3단계를 거치도록 디자인 하는 것이 분할 정복 기법입니다.
1. 분할(divide): 해결이 용이한 단계까지 문제를 분할합니다.
2. 정복(conquer): 분할된 문제를 해결합니다.
3. 결합(combine): 해결된 결과를 결합하여 마무리 합니다.

> ## 예시

예시를 들어 분할 정복이 가지는 이점을 설명하겠습니다.
### 기존
크기가 100인 배열이 존재한다고 가정해 보겠습니다.<br>
이를 O(n^2)의 시간 복잡도가 드는 정렬 알고리즘 사용하여 정렬한다면<br>
총 10000의 횟수가 필요할 것입니다.
### 분할
10000이라는 횟수는 무리가 있으니 배열을 절반씩 나눠줍니다.<br>
50이라는 크기의 배열이 2개 생깁니다.
### 정복(정렬)
각각의 배열에 같은 정렬 알고리즘을 사용한다면<br>
배열당 50^2 = 2500의 횟수가 듭니다. 배열은 두개 존재하므로<br>
5000의 횟수만 필요하다는 것을 알 수 있습니다.
### 결합
두 배열을 다시 합치는 과정이 필요할 것입니다.<br>
총 100칸의 배열을 채워야 하는데 두 배열을 동시에 비교해야 하니<br>
1칸당 2회의 비교 연산을 필요합니다. 즉, 100 * 2 = 200의<br>
횟수가 필요하다는 것을 알 수 있습니다.<br>
<br>
이 세가지 작업을 합쳐서 총 5200의 횟수가 들게 됩니다.<br>
분할하여 정렬하는 것이 기존의 정렬 방식보다<br>
더 적은 연산 횟수를 가진다는 것을 알 수 있습니다.

> ## 과정

합병 정렬의 과정에 대해 얘기해 보겠습니다.<br>
합병 정렬은 위쪽 예시와 과정이 약간 다르게 구현되어 있습니다.<br>
<br>
합병 정렬또한 분할하는 과정을 거치는데<br>
분할된 배열을 정렬(정복)하는 과정은 거치지 않습니다.<br>
대신 분할된 배열의 크기가 1개일 때까지 분할합니다.<br>
<br>
이렇게 한다면 1개의 원소에 대해서는 정렬이 필요가 없어집니다.<br>
허나 정렬하는 과정은 알고리즘에서 반드시 필요할 것입니다.<br>
<br>
위쪽 예시를 보면 분할 과정 뿐만아니라,<br>
결합 과정 또한 정렬이 시행 되는 과정이라는 것을 알 수 있습니다.<br>
합병 정렬은 이러한 특징을 이용하여 배열을 다시 합치는 과정에서 정렬을 시행합니다.<br>
<br>
즉, 합병 정렬은 분할 정복 과정 중,<br>
결합에 초점을 둔 정렬 알고리즘이라 볼 수 있습니다.<br>
합병 정렬이라는 이름이 붙은 이유입니다.
### 순서
다음과 같은 배열을 합병 정렬을 사용하여<br>
오름차순 정렬을 해보도록 하겠습니다.
```
[8 2 3 7 1 5 4 6]
```
먼저 배열의 모든 원소들을 1개가 남을때까지<br>
배열을 절반씩 반복하여 분할해줍니다.
```
[8 2 3 7 1 5 4 6]
[8 2 3 7] [1 5 4 6]
[8 2] [3 7] [1 5] [4 6]
[8] [2] [3] [7] [1] [5] [4] [6]
```
분할작업이 완료되었다면 합병 작업을 시작합니다.<br>
배열을 다시 합병할 때, 작은 원소가 앞에 오도록 정렬합니다.
```
[8] [2] [3] [7] [1] [5] [4] [6]
[2 8] [3 7] [1 5] [4 6]
[2 3 7 8] [1 4 5 6]
[1 2 3 4 5 6 7 8]
```
모든 합병 과정을 마치면 원소들이 정렬되게 됩니다.
> ## 구현

합병 정렬을 구현해 보도록 하겠습니다.<br>
오름차순으로 배열을 정렬하는 병합 정렬 함수입니다.
```cpp
void Merge_Sort(int arr[], int left, int right) {
	if (left >= right)
		return;
	int mid = (left + right) / 2;
	Merge_Sort(arr, left, mid);
	Merge_Sort(arr, mid + 1, right);

	int left_index = left;
	int right_index = mid + 1;
	int* sort_arr = new int[right + 1];
	int sort_index = left;

	while (left_index <= mid && right_index <= right)
		sort_arr[sort_index++] = arr[left_index] < arr[right_index] ? arr[left_index++] : arr[right_index++];

	int extra_index = left_index <= mid ? left_index : right_index;
	while(sort_index <= right)
		sort_arr[sort_index++] = arr[extra_index++];

	for (int i = left; i <= right; ++i)
		arr[i] = sort_arr[i];

	delete[] sort_arr;
}
```
```cpp
void main(void) {
	int arr[10] = { 3, 6, 2, 4, 1, 5, 7, 0, 9, 8 };
	Merge_Sort(arr, 0, 9);
	for (auto iter : arr)
		std::cout << iter;
};
```
합병 정렬 합수의 매개 변수로<br>
정렬할 배열인 arr과 그 배열의 첫번째 인덱스인 left, 마지막 인덱스인 right를 받습니다.<br>
<br>
함수의 코드가 재귀적으로 구성되어 있기 때문에<br>
코드를 몇줄씩 가져와 분석해 보겠습니다.
```cpp
	if (left >= right)
		return;
```
먼저 left와 right가 같다면 원소의 갯수가 한개라는 의미이므로<br>
아무 작업도 수행하지 않고 리턴합니다.<br>
(left가 > right인 경우는 이론상 없습니다.)
```cpp
	int mid = (left + right) / 2;
	Merge_Sort(arr, left, mid);
	Merge_Sort(arr, mid + 1, right);
```
이 코드는 현재 배열을 둘로 나누고 각각의 배열에 대해<br>
병합 정렬 함수를 재귀호출 하는 코드입니다.<br>
<br>
이 과정이 함수의 맨 앞에 와있기 때문에 뒤에오는 코드는 실행되지 않고<br>
원소가 1개인 배열까지 계속해서 나뉘게 됩니다.
```cpp
	int left_index = left;
	int right_index = mid + 1;
	int* sort_arr = new int[right + 1];
	int sort_index = left;
```
배열이 계속해서 나뉘어 졌다면 배열의 원소가 2개일 때부터<br>
이 합병하는 코드가 실행되게 됩니다.<br>
<br>
두 배열의 첫번째 인덱스를 각각 left_index, right_index로 잡고<br>
두 배열의 크기를 합한 sort_arr 배열을 만듭니다.<br>
(코드를 보기 편하게 만들기 위해 타협한 부분이 존재합니다.<br>
배열의 크기를 합한 코드가 아니라 right+1을 받고있습니다.<br>
메모리를 아끼려면 right-left+1이 와야합니다.)
```cpp
	while (left_index <= mid && right_index <= right)
		sort_arr[sort_index++] = arr[left_index] < arr[right_index] ? arr[left_index++] : arr[right_index++];
```
이제 left_index, right_index 둘중 하나의 인덱스가 마지막에 닿을 때까지 sort_arr에<br>
더 작은 값을 채워넣는 과정을 반복하게 됩니다.
```cpp
	int extra_index = left_index <= mid ? left_index : right_index;
	while(sort_index <= right)
		sort_arr[sort_index++] = arr[extra_index++];
```
둘중 하나의 인덱스가 마지막에 닿아서 끝났다면<br>
남은 인덱스를 extra_index에 집어넣고 sort_arr에 마저 채워줍니다.
```cpp
	for (int i = left; i <= right; ++i)
		arr[i] = sort_arr[i];
```
정렬이 완료되었다면 기존 arr에 sort_arr을 집어넣습니다.
> ## 복잡도

### 시간 복잡도
합병 정렬의 시간복잡도는 O(nlog2n)입니다.<br>
이미 정렬되어 있는 경우에도 분할하고 병합하는 과정을<br>
진행하므로 최선, 평균, 최악의 경우에 모두 같은 시간 복잡도를 가집니다.

### 공간 복잡도
합병 정렬의 공간 복잡도는 O(n)입니다.<br>
합병 정렬의 공간 복잡도는 배열 기반이라면 추가적인 메모리를 요구하기 때문에<br>
2n이라고 볼수 있습니다. 허나 빅오에서 이는 무시됩니다.
