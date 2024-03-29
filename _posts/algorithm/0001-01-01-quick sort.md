---
title: "퀵 정렬(Quick Sort)"
categories:
  - algorithm
tags:
  - tag
---
> ## 개요

정렬 알고리즘 중 하나인 퀵 정렬입니다.<br>
배열을 분할하는 과정을 거치면서 정렬하는 알고리즘입니다.<br>
<br>
배열의 원소중 피벗(pivot)을 선정하고 피벗을 기준으로 작은 값은 왼쪽<br>
큰 값은 오른쪽으로 오게한 후, 두 배열에게 다시금 같은 동작을 반복합니다.<br>
<br>
퀵 정렬은 분할 정복(divide and conquer)이라는<br>
알고리즘 디자인 기법에 근거하여 만들어진 정렬 방법입니다.<br>
<br>
일반적인 경우 다른O(nlogn)알고리즘에 비해 매우 빠른 속도로 작동합니다.

> ## 과정

퀵 정렬의 과정에 대해 얘기해 보겠습니다.<br>
<br>
같은 분할 정복 알고리즘인 합병 정렬은 먼저 분할을 시도한 후<br>
분할된 값을 합병하는 과정에서 정렬이 이루어집니다.<br>
<br>
허나 퀵 정렬은 합병 정렬과 다르게 분할이 이루어지는 과정에서<br>
정렬이 이루어집니다.
### 순서
다음과 같은 배열을 퀵 정렬을 사용해<br>
오름차순으로 정렬을 해보겠습니다.
```
[5 1 3 7 9 2 4 6 8]
```
퀵 정렬은 과정이 약간 복잡하기 때문에 수평선으로 단계를 나눠 보겠습니다.

---
#### 준비
먼저 배열의 원소중에 피벗(pivot)에 해당될 원소를 구합니다.<br>
여기서는 가장 간단한 방법인 맨앞 원소인 5를 피벗에 사용해 보겠습니다.
```
[(5) 1 3 7 9 2 4 6 8]
```
피벗이 결정되었다면 피벗의 다음 원소인 1과 맨 마지막 원소인 8을<br>
각각 {low}와 {high}로 선택합니다.<br>
여기까지가 퀵 정렬을 위한 준비 과정입니다.
```
[(5) {1} 3 7 9 2 4 6 {8}]
```

---
#### 탐색과 스왑
이제 low와 high를 각각 탐색하는 과정을 시작합니다.<br>
low는 피벗에 해당되는 값보다 큰 경우가 올때까지 ++을 시키고,<br>
high는 피벗에 해당되는 값보다 작은 경우가 올때까지 --를 시킵니다.
```
[(5) 1 3 {7} 9 2 {4} 6 8]
```
이 과정이 끝났다면 각각 low와 high는 7과 4에 멈추게 됩니다.<br>
이제 이 둘을 스왑하는 과정을 거칩니다.
```
[(5) 1 3 {4} 9 2 {7} 6 8]
```
스왑이 완료되었다면 다시금 탐색 과정을 거칩니다.
```
[(5) 1 3 4 {9} {2} 7 6 8]
```
이번에는 9와 2가 low와 high로 선택되게 됩니다.<br>
다시한번 스왑을 해주도록 하겠습니다.
```
[(5) 1 3 4 {2} {9} 7 6 8]
```

---
#### 분리
위의 탐색과 스왑 과정은 계속해서 반복됩니다.<br>
이제 이를 탈출하는 조건이 필요합니다.<br>
<br>
다시 한번 탐색 과정을 거쳐보겠습니다.<br>
이 때 low는 9를 선택하게 될 것이고, high는 2를 선택하게 될 것입니다.
```
[(5) 1 3 4 {2} {9} 7 6 8] //low는 9고 high는 2입니다.
```
만약 low와 high의 인덱스가 서로 뒤집어지는 상황이 된다면 (low > high)<br>
모든 원소에 대한 탐색과 스왑을 마친 상태라고 볼 수 있습니다.<br>
<br>
이러한 상황이 오면 pivot과 high를 교체합니다.
```
[[2] 1 3 4 {5} {9} 7 6 8]
```
여기까지의 작업에서 알 수 있는 사실은 총 3가지 존재합니다.
1. high(5)를 기준으로 아래 존재하는 원소들은 다 작은 값입니다.
2. high(5)를 기준으로 위에 존재하는 원소들은 다 큰 값입니다.
3. 5는 완벽하게 자신이 정렬될 위치를 찾아 들어갔습니다.

이제 배열을 두개로 나눠서 위와 같은 작업을 진행해 줘야합니다.<br>
5는 이미 정렬되어 있기 때문에 제외합니다.
```
[2 1 3 4] 5 [9 7 6 8]
```
두 배열에 대해 다시금 맨 위로 올라가서 작업하는 과정을 거칩니다.<br>
<br>
윈소가 한개일 때까지 작업하게 된다면<br>
배열의 모든 원소가 정렬되게 됩니다.
> ## 구현

퀵 정렬을 구현해 보겠습니다.<br>
오름차순으로 배열을 정렬하는 퀵 정렬 함수입니다.
```cpp
void Quick_Sort(int arr[], int left, int right) {
	if (left >= right)
		return;

	int pivot = arr[left];
	int low = left + 1;
	int high = right;

	while (low <= high) {
		while (pivot >= arr[low] && low <= right)
			++low;
		while (pivot <= arr[high] && high > left)
			--high;
		if (low > high)
			break;
		int temp = arr[low];
		arr[low] = arr[high];
		arr[high] = temp;
	}
	arr[left] = arr[high];
	arr[high] = pivot;

	Quick_Sort(arr, left, high - 1);
	Quick_Sort(arr, low, right);
}
```
```cpp
void main(void) {
	int arr[10] = { 3, 6, 2, 4, 1, 5, 7, 0, 9, 8 };
	Quick_Sort(arr, 0, 9);
	for (auto iter : arr)
		std::cout << iter;
};
```
퀵 정렬 합수의 매개 변수로<br>
정렬할 배열인 arr과 그 배열의 첫번째 인덱스인 left, 마지막 인덱스인 right를 받습니다.<br>
<br>
함수의 코드가 재귀적으로 구성되어 있기 때문에<br>
코드를 몇줄씩 가져와 분석해 보겠습니다.
```cpp
	if (left >= right)
		return;
```
퀵 정렬은 분할 정복 알고리즘이기 때문에 배열이 계속해서<br>
분할되는 과정을 거칩니다.<br>
<br>
이때 right보다 left가 같거나 크다면 배열이 존재하지 않거나<br>
원소가 한개라는 의미가 되니 정렬 함수를 리턴합니다.
```cpp
	int pivot = arr[left];
	int low = left + 1;
	int high = right;
```
이제 본격적으로 퀵 정렬 함수에 들어갑니다.<br>
pivot과 low, high를 선택해 줍니다. left와 right를 사용하여 간단하게 구현할 수 있습니다.
```cpp
	while (low <= high) {
```
이제 while문을 통해서 탐색하는 과정을 거쳐나갑니다.<br>
low와 high가 서로 교차될 때까지 이 과정을 반복합니다.
```cpp
		while (pivot >= arr[low] && low <= right)
			++low;
		while (pivot <= arr[high] && high > left)
			--high;
```
두 반복문을 통해서 low와 high를 증감시킵니다.<br>
low로 예를 들어서 피벗의 크기보다 큰 상황이 오거나, low가 right를 넘어서는 상황이 올 때까지 진행합니다.
```cpp
		if (low > high)
			break;
		int temp = arr[low];
		arr[low] = arr[high];
		arr[high] = temp;
```
이제 선택된 두 low와 high를 스왑해주어야합니다.<br>
이때 low가 high를 넘어섰다면 탐색이 종료된 상황이니 break문을 통해 스왑을 진행하지 않습니다.
```cpp
	}
	arr[left] = arr[high];
	arr[high] = pivot;
```
while문을 탈출하게 되면 모든 탐색과 스왑 과정이 끝났다는 의미입니다.
이제 high와 pivot를 교체해 줍니다.
```cpp
	Quick_Sort(arr, left, high - 1);
	Quick_Sort(arr, low, right);
```
이제 두개의 배열로 나눠서 같은 작업을 반복해줘야합니다.<br>
피벗보다 작은 배열을 선택하는 방법은 high는 현재 피벗이 들어가있기 때문에 left에서 high까지<br>
피벗보다 큰 배열을 선택하는 방법은 low는 high보다 하나 큰 상황이기 때문에 low에서 right까지<br>
가 됩니다.

> ## 복잡도

### 시간 복잡도
퀵 정렬의 시간복잡도는 O(nlog2n)입니다.<br>
분할 정복 알고리즘을 사용한 정렬이 가지는 시간 복잡도를 지닙니다.<br>
단, 퀵 정렬은 최악의 상황에서 분할되는 배열이 1개가되는 단점이 존재합니다.<br>
따라서 최선, 평균의 시간 복잡도는 O(nlog2n)이지만<br>
최악의 경우 시간 복잡도는 O(n^2)이 됩니다.
### 공간 복잡도
퀵 정렬의 공간 복잡도는 O(n)입니다.
> ## 피벗

### 최악의 상황
퀵 정렬의 최악의 상황에 대해 얘기해보겠습니다.
```
[1 2 3 4 5 6 7 8]
```
만약 다음과 같은 배열에서<br>
피벗을 가장 첫 원소의 값을 잡는다고 가정해 보겠습니다.
```
1 [2 3 4 5 6 7 8]
```
정렬 과정을 거치게 된다면 최종적으로 1은 제 위치를 찾아가고<br>
배열을 두개로 나눠야 하지만 피벗의 아래 값은 없으니 한개만 존재하게 됩니다.<br>
<br>
이 상황을 반복한다면 다음 과정에서는 2 그 다음에는 3 이런식으로<br>
하나씩 원소의 위치를 찾아가게 됩니다.
```
1 2 [3 4 5 6 7 8] ->
1 2 3 [4 5 6 7 8] ->
1 2 3 4 [5 6 7 8] ...
```
한번의 작업에서 배열의 원소중 한개의 위치를 찾기 위해<br>
모든 배열을 탐색하고 있는 모습을 볼 수 있습니다.<br>
<br>
1개의 위치를 잡기위해 n번의 탐색을 n번만큼 반복합니다.<br>
즉, 퀵정렬 최악의 시간 복잡도는 O(n^2)이라 볼 수 있습니다.
### 해결
위 문제는 퀵 정렬에서 피벗을 선택할 때<br>
배열의 중간값을 알 수 없기 때문에 나타나 문제입니다.<br>
<br>
따라서 몇가지 방법을 통해 해당 퀵소트의 문제를 해결할 수 있습니다.
#### 랜덤 피벗
첫번째는 랜덤 피벗 방법입니다.<br>
배열의 맨 앞 원소를 피벗으로 선택하지 않고 랜덤한 원소를 선택하게 만듭니다.
```cpp
	int ran = rand() % (right - left + 1) + left;
	int temp = arr[left];
	arr[left] = arr[ran];
	arr[ran] = temp;
```
랜덤 인덱스를 생성하여 left와 ran의 원소를 스왑해줍니다.<br>
이를 통해 left에는 배열의 원소중 랜덤한 원소가 들어오게 됩니다.
#### Median Of Three Pivot
두번째 방법은 3개의 원소 후보를 두고<br>
그중 중간 값을 선택하는 방법입니다.
```cpp
	int mid = (left + right) / 2;
	int avg = (arr[left] + arr[mid] + arr[right])
		- std::max({ arr[left], arr[mid], arr[right] })
		- std::min({ arr[left], arr[mid], arr[right] });
	int temp = arr[left];
	arr[left] = arr[mid];
	arr[mid] = temp;
```
중간 윈소의 인덱스 mid를 생성하고 평균 원소를 저장할 avg를 생성하여<br>
left, mid, right중 가운데 값을 가져와줍니다.<br>
<br>
두가지 방식 모두 최악의 상황을 막기에 효율적인 방법이지만<br>
어디까지나 예측에 근거한 방법이 됩니다.<br>
우연하게 최악의 경우가 나와버리면 O(n^2)의 시간이 소모됩니다.<br>
<br>
따라서 데이터의 갯수가 많다면<br>
추가적인 메모리 공간이 필요하지만 일관적으로 O(nlog2n)의<br>
시간 복잡도를 보여주는 합병 정렬이 선호됩니다.
> ## 성능

퀵 정렬은 다른O(nlogn)알고리즘에 비해 매우 빠른 속도로 작동합니다.<br>
퀵 정렬의 내부 루프는 컴퓨터 아키텍쳐가 효율적으로 작동하도록 설계되어있습니다.
### 데이터 지역성
CPU가 메모리에 접근하려 할 때,<br>
만약 메모리에 정보가 없다면 디스크에서 메모리로 로드하는 작업을 거치게되고<br>
이 과정은 CPU의 성능 저하를 보이게 됩니다.<br>
<br>
따라서 캐시 메모리는 CPU가 메모리에 접근할 때,<br>
CPU의 행동을 예측하여 미리 메모리를 올려두는 작업을 보입니다.<br>
<br>
CPU가 필요한 메모리와 캐시가 예측한 메모리가 적중하였다면<br>
이를 Hit했다고 표현합니다.<br>
<br>
데이터 지역성이란<br>
CPU가 빠른 시간 내에 일정 구간의 메모리 영역을 반복적으로 접근하는 경향을 의미합니다.<br>
따라서 캐시 메모리의 효율을 올리기 위해 데이터 지역성에 근거해 캐시 메모리를 사용하게 됩니다.<br>
<br>
데이터 지역성은 다음 3가지로 분류됩니다.
- 시간(Temporal) 지역성 : 최근 사용된 Page를 다시 참조할 확률이 높습니다.
- 공간(Spatial) 지역성 : 최근 사용된 Page의 근처 Page들이 다시 참조될 확률이 높습니다.
- 순차(Sequential) 지역성 : 데이터가 순차적으로 접근되는 경향으로 프로그램 명령어가 순차적으로 구성되어 있는 경우입니다.

이 지역성의 원리에 따라 두가지 정렬을 판단해 본다면,<br>
<br>
합병 정렬은 먼저 분할을 통해 배열을 쪼갠 후 각 배열에 접근하여 정렬을 시도하기 때문에<br>
각 단계마다 모든 배열의 범위를 순차적으로 돈다는 특징을 가지고 있습니다.<br>
<br>
반대로 퀵 정렬은 분할을 거치기 전에 정렬을 시도하고 배열이 쪼개진다 해도<br>
쪼개진 배열 내부로 접근하는 방식을 취하고 있습니다.<br>
이는 퀵 정렬이 한번에 넓은 범위를 움직이지 않는다는 의미가 됩니다.<br>
<br>
따라서 데이터 지역성, 캐시 적중률이 높은 쪽은 퀵 정렬이 됩니다.
### 꼬리 재귀 제거
영어로 Tail Recursion Elimination 방식이라고 불립니다.<br>
이를 이해하기 위해서 몇가지 용어를 먼저 알아야합니다.
1. Tail Call

어떤 함수가 실행될 때 리턴 명령어를 만나기 직전에 특정 함수가 호출되는 것을 의미합니다.<br>
현재 함수의 스택 프레임의 할당 해제가 일어나기 직전에 수행되는 경우를 의미합니다.
```cpp
return function(--num); // 이것은 Tail Call 입니다. 
return function(num--); // 이것은 Non-Tail Call이 아닙니다, 함수 실행 후 --연산 추가 실행!
```
2. Tail Recursion

Tail Call의 특수한 경우로 Tail Call이 재귀 호출로 일어나는 경우를 의미합니다.
```cpp
void function(int num){
	return function(--num);
}
```
3. TCO(Tail Call Optimization)

Tail Call을 최적화 하는 작업을 의미합니다.<br>
컴파일러 차원에서 지원됩니다. 지원하지 않는 컴파일러도 존재합니다.<br>
Tail Call 호출에서 함수의 새 스택 프레임을 생성하지 않고<br>
현재 함수에서, 호출된 함수의 진입점으로 goto하여 함수를 진행할 수 있게 만듭니다.

4. Stack Frame

스택 프레임은 함수의 호출시 메모리 스택에<br>
함수의 매개 변수, 반환 주소값, 지역 변수등이 저장되는데<br>
이러한 스택 영역에 저장되는 정보를 가리켜 스택 프레임이라고 합니다.<br>
<br>
이것을 사용하여 함수가 중첩 호출 되어도 이전 상태로 돌아가는 작업이 가능합니다.<br>
허나 함수의 호출이 자주 중첩되다 보면 스택 프레임이 쌓이게 될 경우<br>
스택에 메모리가 가득 차게되어 StackOverFlow를 발생시킵니다.<br>
<br>
TCO는 이러한 문제를 해결하기 위한 최적화로 현재 함수의 스택 프레임 값만 대체하여<br>
별도 공간을 사용하지 않고 재사용 하게 만드는 최적화 방법입니다.<br>
<br>
그 중에서도 재귀 형태의 Tail Recursion을 TCO한 것을 가리켜<br>
Tail Recursion Elimination이 적용된다고 부릅니다.
```cpp
void function(int num) {
	if (num < 0)
		return;
	std::cout << num << " ";
	function(num - 1);
}
```
```cpp
static void function(int num) {
start:
	if (num < 0)
		return;
	std::cout << num << " ";
	num = num - 1;
	goto start;
}
```
이를 퀵 정렬 함수에 사용하면 이렇게 됩니다.
```cpp
void Quick_Sort(int arr[], int left, int right) {
start:
	if (left >= right)
		return;

	int pivot = arr[left];
	int low = left + 1;
	int high = right;

	while (low <= high) {
		while (pivot >= arr[low] && low <= right)
			++low;
		while (pivot <= arr[high] && high > left)
			--high;
		if (low > high)
			break;
		int temp = arr[low];
		arr[low] = arr[high];
		arr[high] = temp;
	}
	arr[left] = arr[high];
	arr[high] = pivot;

	Quick_Sort(arr, left, high - 1);
  
	left = low;
	right = right;
	goto start;
}
```
