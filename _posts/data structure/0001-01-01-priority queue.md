---
title: "우선순위 큐(Priority Queue)"
categories:
  - data structure
tags:
  - tag
---
> ## 개요

큐의 삽입 순서에 관계 없이 우선순위에 따라 값을 내보내는 큐를 의미합니다.<br>
우선순위 큐는 추상적 자료 구조의 개념을 의미합니다.<br>
<br>
우선순위 큐는 배열 또는 연결 리스트로도 구현할 수 있지만<br>
대부분 힙(heap)을 통해 구현됩니다.
> ## 성능

우선순위 큐의 구현에<br>
배열 또는 연결 리스트보다 힙이 적합한 이유는<br>
다음과 같은 성능 차이 때문이라고 할 수 있습니다.<br>
<br>
우선순위 큐는 데이터가 삽입되었을 때, 우선순위에 따라 정렬되야하는 과정을 거칩니다.<br>
이를 위해 이미 지니고 있는 데이터와의 검사가 필요합니다.<br>
<br>
만약 배열 또는 연결 리스트로 우선순위 큐를 구현하였다면<br>
순회를 통해 데이터와 우선순위 검사를 해야하는 과정을 거칩니다.<br>
이 때 우선순위가 가장 낮은 데이터가 삽입되었다면,<br>
모든 데이터와 검사를 진행할 것입니다.<br>
<br>
힙으로 우선순위 큐를 구현하였다면<br>
마찬가지로 데이터와 우선순위 검사를 진행해야 하지만,<br>
이진트리의 특성에 따라 검사를 진행하는 데이터의 양이 한번 진행될 때마다 절반씩 줄어들게 됩니다.<br>
<br>
이들의 시간 복잡도를 나타내면 다음과 같습니다.
- 배열/연결 리스트 시간 복잡도: O(n)
- 힙 트리 시간 복잡도: O(log2n)

따라서 배열이나 리스트 보다 힙으로 구현하는것이<br>
매번 정렬을 실행해야되는 우선순위 큐의 특성상 더 적합합니다.
> ## 구조

힙과 우선순위 큐는 거의 똑같은 구조를 가집니다.<br>
한가지 다른 것이 우선순위를 결정하는 방법입니다.<br>
<br>
힙은 데이터의 값에 따라 최소 힙과 최대 힙이 존재할 수 있습니다.<br>
하지만 데이터의 값이 꼭 우선순위라는 보장은 없을 것입니다.<br>
<br>
우선순위 큐는 이러한 힙의 한계에 우선순위를 판단할 수 있는 과정을 추가합니다.
> ## 구현

어떻게 우선순위를 결정할 수 있는지 구현해 보겠습니다.<br>
힙을 기반으로 만들어져 있기 때문에 힙을 알고 있는것이 좋습니다.<br>
<br>
힙과의 차이점을 중심으로 설명하겠습니다.
### 클래스
```cpp
template<typename _Ty, typename _Pr = Less<typename _Ty>>
class Priority_Queue final {
public:
	explicit Priority_Queue(void);
	~Priority_Queue(void);
public:
	void Push(const _Ty& _data);
	void Pop(void);
	_Ty Top(void);
	size_t Size(void);
private:
	_Ty* _array;
	_Pr _comp;
	size_t _size = 0;
};
```
우선순위 큐 클래스입니다.<br>
힙 클래스에서 추가된 점은 우선순위를 판단할 수 있는 Pr 템플릿이 추가되었다는 것입니다.<br>
<br>
Pr 템플릿은 우선순위를 판단할 수 있는 펑터(Functor)를 받게됩니다.<br>
펑터를 저장할 변수로 comp가 존재합니다.<br>
기본적으로 오름차순(Less) 펑터를를 받게됩니다.<br>
<br>
이를 사용하여 멤버 함수들에서 우선순위를 판단합니다.
### 구현부
```cpp
template<typename _Ty, typename _Pr>
Priority_Queue<_Ty, _Pr>::Priority_Queue(void) :
	_array(static_cast<_Ty*>(calloc(sizeof(_Ty) * 128, sizeof(_Ty) * 128))) {
}

template<typename _Ty, typename _Pr>
Priority_Queue<_Ty, _Pr>::~Priority_Queue(void) {
	free(_array);
}

template<typename _Ty, typename _Pr>
void Priority_Queue<_Ty, _Pr>::Push(const _Ty& _data) {
	size_t child = ++_size;

	while (child != 1) {
		size_t parent = child / 2;

		if (_comp(_array[parent], _data)) //대소 비교를 펑터에게 맏깁니다.
			break;
		_array[child] = _array[parent];
		child = parent;
	}
	_array[child] = _data;
}

template<typename _Ty, typename _Pr>
void Priority_Queue<_Ty, _Pr>::Pop(void) {
	size_t parent = 1;

	_Ty leaf = _array[_size--];

	while (true) {
		size_t left = parent * 2;
		size_t right = left + 1;
		size_t child = 0;
		if (left > _size)
			break;
		else if (left == _size)
			child = left;
		else
			child = _comp(_array[left], _array[right]) ? left : right; //대소 비교를 펑터에게 맏깁니다.

		if (_comp(leaf, _array[child])) //대소 비교를 펑터에게 맏깁니다.
			break;
		_array[parent] = _array[child];
		parent = child;
	}

	_array[parent] = leaf;
}

template<typename _Ty, typename _Pr>
_Ty Priority_Queue<_Ty, _Pr>::Top(void) {
	return _array[1];
}

template<typename _Ty, typename _Pr>
size_t Priority_Queue<_Ty, _Pr>::Size(void) {
	return _size;
}
```
한번에 작성한 이유는 번경된 부분이 딱 3부분이기 때문입니다.<br>
<br>
각 함수에서 대소 비교를 하는 과정을 comp 펑터에게 맏기게 됩니다.<br>
기존에 있던 data1 < data2 의 자리에 comp(data1, data2)가 들어갔습니다.<br>
<br>
이제 펑터를 변경하기만 하면 어떤 우선순위도 처리할 수 있게 되었습니다.
#### 펑터
```cpp
template<typename _Ty>
struct Less final {
public:
	bool operator()(const _Ty& a, const _Ty& b) {
		return a < b;
	}
};

template<typename _Ty>
struct Greater final {
public:
	bool operator()(const _Ty& a, const _Ty& b) {
		return a > b;
	}
};
```
우선순위 비교를 하기 위해 펑터를 구현하였습니다.<br>
기본적으로 오름차순과 내림차순을 담당하는 Less, Greater 펑터가 존재합니다.<br>
<br>
자신만의 펑터를 제작하여 대체하여 원하는 우선순위를 정할 수 있습니다.
> ## 사용

구현된 우선순위 큐를 사용해 보겠습니다.<br>
<br>
자신만의 데이터 구조체를 구현하고,<br>
이 구조체의 우선순위를 파악하기 위한 펑터를 제작한 후<br>
둘을 사용하여 메인 함수를 작성해 보겠습니다.
### 구조체
```cpp
struct People final {
	explicit People(int age) :
		_age(age) {
	}
	int _age = 0;
};
```
구조체 입니다.<br>
사람에 대한 정보는 다양하게 존재할 수 있습니다.<br>
그 중에서 나이를 기반으로 우선순위 큐를 구현하고자 합니다.

### 펑터
```cpp
template<typename _Ty>
struct  Functor final {
public:
	bool operator()(const _Ty& a, const _Ty& b) {
		return a._age > b._age;
	}
};
```
펑터 입니다.<br>
나이를 맴버 변수로 꺼내와 비교하는 연산을 진행합니다.<br>
가장 높은 나이 순으로 정렬되게 됩니다.<br>
이 두가지를 사용하여 메인함수를 구현하겠습니다.

### 메인 
```cpp
void func(void) {
	Priority_Queue<People, Functor<People>> priority_queue;
	priority_queue.Push(People{ 60 });
	priority_queue.Push(People{ 30 });
	priority_queue.Push(People{ 40 });
	priority_queue.Push(People{ 10 });
	priority_queue.Push(People{ 50 });
	priority_queue.Push(People{ 20 });

	while (0 != priority_queue.Size()) {
		std::cout << priority_queue.Top()._age << std::endl;
		priority_queue.Pop();
	}
}
```
우선순위 큐를 생성할 때 템플릿 인자로 펑터를 전달해 주었습니다.<br>
이를 통해 우선순위 큐는 나이가 높은 순으로 우선순위를 잡을 수 있게 되었습니다.
