---
title: "시간 복잡도(Time Complexity)"
categories:
  - algorithm
tags:
  - tag
---
> ## 개요

알고리즘의 연산 수행속도를 측정하는 수식을 나타냅니다.<br>
데이터의 수 n에 따른 연산 횟수의 관계를 나타내는 함수 T(n)을 표현합니다.
> ## 이론

시간 복잡도를 이해하기 앞서<br>
자료구조와 알고리즘에 대한 몇가지 사전 지식이 필요합니다.

---
### 자료 구조
프로그래밍을 하는 과정을 한마디로 요약하면<br>
데이터를 저장하고 저장된 데이터를 활용하여 어떤 처리를 하는 것이라고 볼 수 있습니다.<br>
<br>
여기서 데이터를 저장하는 과정을<br>
우리는 자료 구조라는 정보의 집합체에 담는 방법을 선택합니다.
```cpp
void main(void) {
	int arr[5]{ 0, 1, 2, 3, 4 };
}
```
배열 자료 구조를 사용하여 int형 데이터를 저장해 보았습니다.

---
### 알고리즘
자료 구조에 데이터를 담았다면 이를 처리하는 과정이 필요합니다.<br>
데이터를 처리하는 과정에는 이 자료 구조에 대한 접근이 동반될 것입니다.
```cpp
void main(void) {
	int arr[5]{ 0, 1, 2, 3, 4 };

	int  search = 2;
	for (int i = 0; i < 5; ++i)
		if (search == arr[i]) {
			std::cout << "찾았다!" << std::endl;
			break;
		}
};
```
배열의 인덱스중 2를 찾으면 찾았다라고 외칩니다.<br>
이를 배열에 대한 탐색 알고리즘이라 부릅니다.<br>
<br>
이처럼 알고리즘과 자료구조는 떼어질 수 없는 관계를 가집니다.<br>
배열이라는 자료구조가 결정되어 있어서 순차 탐색이라는 알고리즘을 결정할 수 있습니다.<br>
<br>
자료 구조는 배열 한가지만 존재하지 않습니다.<br>
만약 배열이 아니였다면 탐색 방법은 달라졌어야 합니다.<br>
<br>
또한 자료 구조가 같다 할지라도, 알고리즘은 방식은 다양할 수 있습니다.<br>
위 코드는 순차 탐색이지만 이진 탐색 방식도 고려해 볼 수 있겠습니다.<br>
아래서 이야기할 내용은 이 탐색 알고리즘들에 관련된 것입니다.<br>

---
### 시간 복잡도(time complexity)와 공간복잡도(space complexity)
자료구조가 여러 가지 일 때<br>
그에 맞는 알고리즘을 제대로 채택했다고 가정해보겠습니다.<br>
<br>
그러면 그에 맞는(자료 구조에 맞는) 알고리즘이 여러가지 일 때는<br>
무슨 기준으로 알고리즘을 채택해야 할까요?<br>
<br>
알고리즘의 최우선 순위는 가장 효율적인 알고리즘을 채택하는 것이라 볼 수 있습니다.<br>
여기서 효율적인 알고리즘을 판단하는 요소로 2가지를 볼 수 있습니다.
- 어떤 알고리즘이 가장 빠르고 또 느린가요?
- 어떤 알고리즘이 메모리를 적게 혹은 많이 쓰나요?

하나는 속도에 관련된것이고 다른 하나는 메모리 사용량에 관련된 것입니다.<br>
이를 이렇게 표현합니다.
- 시간 복잡도: 알고리즘에 들어가는 수행시간 분석 결과
- 공간 복잡도: 알고리즘에 들어가는 메모리 사용량 분석 결과

가장 최선책은 속도도 빠르고 메모리도 적게 쓰는 것이겠지만<br>
두가지가 모두 베스트인 경우는 드뭅니다.<br>
<br>
일반적으로 알고리즘의 효율성을 따질때는 메모리보다는 속도에 초점을 둡니다.<br>
하드웨어가 발전함에 따라 메모리의 여유공간은 점점 커져간것도 있고<br>
사용자는 메모리를 체감하지 못하지만, 속도는 체감하기 때문입니다.<br>

---
### 시간 복잡도 계산
이제 시간 복잡도가 중요하다는 점을 알았으니<br>
시간 복잡도를 계산하여 가장 효율적인 알고리즘을 채택해야 합니다.<br>
<br>
그렇다면 어떻게 계산해야 할까요?<br>
시계를 가져다 두고 일일히 시간을 재고있을수는 없는 노릇일 것입니다.<br>
<br>
그냥 대충좀 넣어보고 빠른거 고르면 안되나요?<br>
여기서 한가지 더 알고 가야할 사실이 있는데<br>
알고리즘은 처리해야될 데이터의 양에 따라 수행 시간이 변경된다는 것입니다.<br>
<br>
이게 무슨소리냐면
1. 데이터가 50개일 때 5초 걸리던 알고리즘이, 데이터가 500개가 되면 500초 걸릴 수도 있고
2. 데이터가 50개일 때 10초 걸리던 알고리즘이, 데이터가 500개가 되면 100초 걸릴 수도 있다는 것입니다.

이러한 상황에서 시계로 계산하려 들면 몇번의 시간 계산으로 안됩니다.<br>
데이터 양의 변화에 따라 걸리는 시간의 변화를 알아야하는데 이를 위해<br>
수십 수백 수천번을 계산하고 있을 수는 없을 것입니다.<br>
<br>
따라서 알고리즘의 시간 복잡도를 계산하기 위해서 다음과 같은 방식을 취합니다.
- 시간을 재는 대신 연산의 횟수를 세기로 합니다.
- 데이터 n개에 대한 연산 횟수를 계산하는 함수 T(n)을 구성합니다.

시간으로 계산하기에는 무리가 있는걸 알았으니까 연산횟수 = 시간이라는 정의를 내립니다.<br>
그리고 데이터 n개에 따른 연산 횟수를 계산하는 함수T(n)를 세워<br>
알고리즘의 빠르기를 판단하는 것입니다.<br>
<br>
데이터의 개수 n을 함수에 넣으면 결과로 연산 횟수를 반환하는데 이렇게 하면<br>
데이터의 개수에 따른 연산횟수의 변화 정도를 판단할 수 있습니다.<br>

---
### 연산 횟수
시간 복잡도를 계산하기 위해 연산 횟수라는 개념을 사용한 것은 보았습니다.<br>
그렇다면 연산 횟수는 어떤것을 가리는 말일까요?
```cpp
	for (int i = 0; i < 5; ++i)
		if (search == arr[i]) {
			std::cout << "찾았다!" << std::endl;
			break;
		}
```
아까 사용했던 탐색 코드의 일부분입니다.<br>
여기서 사용된 반복되는 연산자는 <, ++, == 입니다.<br>
<br>
여기서 어떠한 연산자가 연산 횟수를 가리킬까요?<br>
탐색 알고리즘은 == 연산자가 중점인 알고리즘입니다.<br>
따라서 == 연산자를 연산 횟수라고 칭할 수 있습니다.

---
### 데이터의 개수
마지막으로 한가지가 더 남았는데 바로 n의 개수에 관련된 것입니다.<br>
앞선 예시를 살펴보면 n이 적을때는 1번이, n이 많을때는 2번이 적합한 알고리즘일 것입니다.<br>
<br>
그렇다면 2번이 적합한 알고리즘일까요?<br>
사실 여기에 대한 정답은 없습니다.<br>
상황에 따라 n이 몇개인지 수식은 알 수 없습니다.

---
### 최선의 경우(best case)와 최악의 경우(worst cast)
이제 효율적인 알고리즘을 선택하는 수식을<br>
생성하여 알고리즘을 판단할 수 있게 되었습니다.<br>
<br>
여기서 다시한번 짚고 넘어가야할 것이 있습니다.<br>
위에서 얘기했던 모든 알고리즘은 탐색 알고리즘이라는 것입니다.<br>
<br>
위에서 얘기했던 예시를 가져와 보겠습니다.<br>
연산 횟수에 대해 알았으니 연산 횟수로 바꿔보겠습니다.
1. 데이터가 50개일 때 5회 걸리던 알고리즘이, 데이터가 500개가 되면 500회 걸린다.
2. 데이터가 50개일 때 10회 걸리던 알고리즘이, 데이터가 500개가 되면 100회 걸린다.

의문점이 하나 존재합니다.<br>
1번 방식을 사용했을 때 데이터가 500개인 경우라도<br>
찾고싶어하는 데이터가 한번에 나왔으면 1회 아닐까요?<br>
500회라는 연산 횟수은 어떻게 측정된 것일까요?<br>
<br>
시간복잡도 계산을 설명하던 와중에 저것을 설명하기에는<br>
글이 너무 복잡해지기 때문에 뒤로 미뤄두었습니다.<br>
<br>
탐색 알고리즘의 경우
- 운이 좋아서 탐색할 대상이 바로 등장하는 경우가 있고
- 운이 없어서 탐색할 대상이 탐색 알고리즘의 맨 뒤에 존재하거나 아예 없는 경우도 있을 것입니다.

이러한 경우를 최선의 경우와 최악의 경우라 부릅니다.<br>
<br>
알고리즘의 효율성을 계산하는데 있어서 최선의 경우는 고려하지 않습니다.<br>
왜냐면 최선의 경우에는 대게 만족할만한 결과를 보여주기 때문입니다.<br>
반면 알고리즘의 최악의 경우에는 알고리즘 별로 성능에 큰 차이를 보이는 경우가 많습니다.<br>
따라서 위 예시의 500회라는 연산 횟수는 최악의 경우를 계산한 것입니다.<br>

> ## 실전

이제 이론에서 설명한 시간 복잡도를 직접 계산해봐야 할 것 같습니다.
- 데이터 n개에 대한
- 알고리즘 최악의 경우의
- 연산 횟수인 == 연산자를
- 계산할 수 있는 수식T(n)으로 나타낸다.

이를 위해 예제가 필요할 것입니다.
### 순차 탐색
순차 탐색을 사용했던 위 예시를 먼저 사용해 보겠습니다.
```cpp
void main(void) {
	int arr[5]{ 0, 1, 2, 3, 4 };

	int  search = 3;
	for (int i = 0; i < 5; ++i)
		if (search == arr[i]) {
			std::cout << "찾았다!" << std::endl;
			break;
		}
};
```
그런데 계산 할것이 없습니다.<br>
이 코드의 흐름을 파악했다면 다음 사실을 바로 파악하였기 때문입니다.<br>
데이터가 n개일 때, 최악의 경우에 해당하는 연산 횟수는 n회이다.<br>
<br>
이것을 수식으로 세우면 다음과 같습니다.
```
T(n) = n
```
너무 간단한것 같지만 어찌됐든 이것이 순차 탐색의 시간 복잡도 입니다.
### 이진 탐색
이번에는 이진 탐색을 수식으로 나타내 보겠습니다.<br>
(이진 탐색에 대해 미리 알고있다는 가정하에 작성하겠습니다.)<br>
<br>
먼저 코드를 보겠습니다.
```cpp
int Search(int arr[], int len, int search) {
	int first = 0;
	int last = len - 1;

	while (first <= last) {
		int mid = (first + last) / 2;

		if (search == arr[mid])
			return mid;
		else {
			if (search < arr[mid])
				last = mid - 1;
			else
				first = mid + 1;
		}
	}
	return -1;
}
```
```cpp
void main(void) {
	int arr[5]{ 0, 1, 2, 3, 4 };

	int result = Search(arr, 5, 1);

	std::cout << result << std::endl;
};
```
이진 탐색을 사용하는 함수를 제작하였습니다.<br>
자 이 코드에서 다시 한번 시간복잡도를 계산해 보겠습니다.<br>
<br>
데이터가 n개일 때, 최악의 경우에 해당하는 연산 횟수는 몇회인가?<br>
이진 탐색 트리의 경우 시간 복잡도가 어렵기 때문에<br>
답이 딱 나오지 않습니다.<br>
<br>
따라서 직접 한번 세어보겠습니다.
1. 데이터 수가 n개일 때 1회 연산
2. 데이터 수가 n/2개 일 때 1회 연산
3. 데이터 수가 n/4개 일 때 1회 연산
...
4. 데이터 수가 1개일 때 1회 

이런 순서를 보입니다.<br>
이렇게 봐서는 한눈에 들어오지 않으니 대입을 한번 해보겠습니다.<br>
데이터의 갯수 n은 8입니다.
1. 데이터 수가 8개일 때 1회 연산
2. 데이터 수가 4개일 때 1회 연산
3. 데이터 수가 2개일 때 1회 연산
4. 데이터 수가 1개일 때 1회 연산

이번엔 눈에 조금 들어오기 시작합니다.<br>
8이라는 숫자가 1이되기까지 2로 나눈 횟수는 총 3번이고 이때마다 연산을 1회 진행했습니다.<br>
마지막으로 남은 데이터 1에서 연산한 횟수 1회를 추가합니다.<br>
<br>
정리하자면<br>
데이터의 갯수가 1이 될때 까지 나눈 횟수 k회 연산 k회 진행<br>
데이터가 1개 남았을 때 마지막으로 연산 1회 진행<br>
<br>
이를 시간복잡도 수식으로 표현해보겠습니다.
```
T(n) = k + 1
```
이제 여기서 k를 구해야하는데 k는 다음과 같이 나타낼 수 있습니다.
```
n * (1/2)^k = 1
```
이제 이 수식을 n을 표현할 수 있게 정리해보겠습니다.
```
n * (1/2)^k = 1 >> n * 2^(-k) >> n = 2^k

n = 2^k >> log2n = log2 2^k >> log2n = k log22 >> log2n = k
```
결론은 k = log2n이라고 볼 수 있습니다.<br>
따라서 이진 탐색 알고리즘에 대한 시간 복잡도는 다음과 같습니다.
```
T(n) = log2n + 1
```
