---
title: "원형 연결 리스트(Circular Linked List)"
categories:
  - data structure
tags:
  - tag
---
> ## 구현

원형 연결 리스트를 구현해보도록 하겠습니다.<br>
<br>
원형 연결 리스트는 단일 연결 리스트의<br>
개량 혹은 파생이라고 볼 수 있을 정도로 비슷합니다.<br>
<br>
따라서 단일 연결 리트스와 많은 코드가 중복되기 때문에 설명보다 구현에 초점을 두고<br>
단일 연결 리스트와의 차이점에 대해서만 자세히 설명하겠습니다.<br>
### 구조체
연결할 노드 구조체를 구현해보겠습니다.<br>
원형 연결 리스트는 지속적인 순회를 해야되기 때문에 대부분 더미 노드를 대개 지원하지 않습니다.
```cpp
template<typename _Ty>
struct Node final {
	explicit Node(const _Ty& value) :
		_value(value) {
	}
	_Ty _value;
	Node* _next = nullptr;
};
```

### 클래스
다음은 원형 연결 리스트 클래스입니다.<br>
단일 연결 리스트와 유사하지만 몇가지 차이점이 존재합니다.
```cpp
template<typename _Ty>
class List final {
public:
	using Iterator = Iterator<_Ty>;
public:
	explicit List(void);
	~List(void);
public:
	void Push_Front(const _Ty& value);
	void Push_Back(const _Ty& value);
	void Pop_Front(void);

	Iterator Emplace(const Iterator& iter, const _Ty& value);
	Iterator Erase(const Iterator& iter);

	Iterator Begin(void);

	size_t Size(void);
private:
	Node<_Ty>* _tail = nullptr;
	size_t _size = 0;
};
```
먼저 리스트는 노드의 head를 참조하지 않고 tail을 참조합니다.<br>
이유는 더미 노드가 없기 때문에 head를 참조한다면 prev를 참조할 수 없습니다.<br>
따라서 노드의 머리를 가져오고 싶을 때는 tail.next를 참조할 수 있게 만듭니다.<br>
<br>
이를 변형된 원형 연결 리스트라고 부릅니다.<br>
변형본이지만 원본 모델보다 더 일반적인 모델로 인식되고 있습니다.<br>
<br>
이를 통한 이점은<br>
더미 노드를 사용하지 않고도 머리와 꼬리 부분에 추가할 수 있게 되었다는 점이 있습니다.<br>
함수를 보면 단일 연결 리스트는 push_front밖에 지원을 하지 않지만<br>
원형 연결 리스트는 push_back또한 지원 가능합니다.
### 크기
단일/이중 연결 리스트와 차이점이 없습니다.<br>
리스트의 삽입/삭제 함수의 끝에 size의 증감 연산을 추가합니다.
```cpp
template<typename _Ty>
size_t List<_Ty>::Size(void) {
	return _size;
}
```
### 삽입
다음은 삽입 함수입니다. 단일/이중 연결 리스트와 다르게 더미 노드가<br>
존재하지 않기 때문에 생성자는 작성하지 않겠습니다.<br>
원형 연결 리스트의 경우 다음과 같은 상황에 삽입을 지원해야 합니다.
1. 리스트의 머리와 꼬리에 노드를 삽입할 수 있습니다.
2. 리스트의 중간 부분에 노드를 삽입할 수 있습니다.

#### 머리/꼬리 삽입
먼저 리스트의 머리와 꼬리에 삽입하는 코드를 작성해 보겠습니다.
```cpp
template<typename _Ty>
void List<_Ty>::Push_Front(const _Ty& value) {
	Emplace(Begin(), value);
}

template<typename _Ty>
void List<_Ty>::Push_Back(const _Ty& value) {
	Iterator iter = Emplace(Begin(), value);
	_tail = *iter;
}
```
머리를 참조하는 반복자를 만드는 함수 begin을 호출하고<br>
이 후 emplace를 호출하여 머리 부분에 삽입을 진행하였습니다.<br>
<br>
여기서 원형 연결 리스트가 꼬리 삽입을 지원하는 방법을 알 수 있습니다.<br>
push_back 함수를 보면 머리 부분에 삽입을 진행한 다음<br>
반환되는 반복자는, 집어넣은 노드 즉 head에 해당합니다. 이를 tail로 바꿈으로써<br>
꼬리에 삽입한 효과를 볼 수 있는 것입니다.
#### 중간 삽입
다음은 중간 삽입 코드입니다. 더미 노드를 사용하지 않음으로써<br>
몇가지 조건문이 추가되었다는 것을 볼 수 있습니다.
```cpp
template<typename _Ty>
Iterator<_Ty> List<_Ty>::Emplace(const Iterator& iter, const _Ty& value) {
	Node<_Ty>* prev = iter.Prev();
	Node<_Ty>* node = new Node<_Ty>{ value };
	Node<_Ty>* cur = (*iter);

	if (nullptr == _tail)
		_tail = prev = cur = node;

	prev->_next = node;
	node->_next = cur;

	++_size;
	return Iterator{ prev, node };
}
```
prev와 cur사이에 node를 끼워넣는 코드는 똑같습니다.<br>
이를 위해 prev, node, cur의 관계를 설정하는 코드가 동작합니다.<br>
<br>
하지만 더미 노드가 없기 때문에 아예 비어있을 때가 존재하게 됩니다.<br>
<br>
이를 판단하는 조건문으로 tail이 nullptr이라면이 동작할 수 있고<br>
이 때 tail, prev, cur를 전부 node로 바꿔주는 코드가 있습니다.<br>
이렇게 하면 이후 코드는 tail의 next를 tail로 설정하는 코드가 됩니다.
### 삭제
삭제를 지원하기 위해 삭제 코드를 작성해 보겠습니다.<br>
삭제코드는 삽입 코드와 다르게 단일 연결 리스트처럼 push_front밖에 지원하지 못합니다.<br>
삭제를 지원해야 하는 경우는 다음과 같습니다.
1. 리스트의 머리 노드를 삭제할 수 있어야 합니다.
2. 리스트의 중간 노드를 삭제할 수 있어야 합니다.
3. 리스트가 소멸할 때 모든 노드를 삭제해야 합니다.

#### 머리 삭제
머리 삭제 함수입니다. 단일 연결 리스트와 다름 없이<br>
코드의 재사용을 위해 erase를 호출합니다. 인수로 머리를 참조하는 반복자를 반환하는 begin함수를 호출합니다.
```cpp
template<typename _Ty>
void List<_Ty>::Pop_Front(void) {
	Erase(Begin());
}
```
#### 중간 삭제
중간 삭제 함수입니다.<br>
이 곳에도 마찬가지로 더미 노드가 존재하지 않기 때문에 작성되는 조건문이 있습니다.
```cpp
template<typename _Ty>
Iterator<_Ty> List<_Ty>::Erase(const Iterator& iter) {
	Node<_Ty>* cur = (*iter);
	if (nullptr == cur)
		return Iterator{};
	Node<_Ty>* prev = iter.Prev();
	Node<_Ty>* next = cur->_next;

	prev->_next = next;
	if (cur == _tail)
		if (prev == _tail)
			_tail = nullptr;
		else
			_tail = prev;

	delete cur;
	--_size;
	return Iterator{ prev, next };
}
```
기본 코드는 단일 연결 리스트와 마찬가지로 prev, cur, next를 참조하고<br>
cur를 삭제하며 prev와 next를 연결하는 코드입니다.<br>
허나 이때 2가지 예외 사항이 발생할 수 있습니다.
- cur가 tail일 수 있습니다.
- cur가 tail인데다가 리스트에 남은 노드가 tail 한개일 수 있습니다.

두번째 예외 상황은 첫번째 예외사항을 포함하고 있습니다.<br>
따라서 중첩 if문으로 이 상황을 처리 할 수 있습니다.<br>
<br>
cur가 tail 이라면 tail이 삭제되는 경우이니 그 이전 노드인 prev로 tail을 옮겨줍니다.<br>
허나 이때 prev또한 tail이라면 남은 노드가 한개였다는 뜻이 되니 tail을 nullptr로 바꿔줍니다.
#### 소멸자
소멸자의 구현은 더 간단해 집니다.<br>
더미 노드가 존재하지 않기 때문에 tail이 nullptr일 때 까지 pop_front를 계속 호출해줍니다.
```cpp
template<typename _Ty>
List<_Ty>::~List(void) {
	while (nullptr != _tail)
		Pop_Front();
}
```
### 탐색
단일 연결 리스트에서는 탐색을 위해 반복자를 사용하였습니다.<br>
여기서 사용되는 반복자와 티끌하나 다르지 않으니 탐색 코드는 생략하겠습니다.
> ## 사용
구현한 원형 연결 리스트를 사용해 보겠습니다.
```cpp
void main(void) {
	List<int> list;
	List<int>::Iterator iter;

	list.Push_Front(30);
	list.Push_Back(40);
	list.Push_Front(20);
	list.Push_Back(50);
	list.Push_Front(10);

	list.Pop_Front();
	list.Pop_Front();

	iter = list.Begin();
	++iter;
	iter = list.Emplace(iter, 1000);
	iter = list.Erase(iter);

	iter = list.Begin();
	for (int i = 0; i < list.Size(); ++i) {
		std::cout << (*iter)->_value << std::endl;
		++iter;
	}

	while (true) {
		std::cout << (*iter)->_value << std::endl;
		++iter;
	}
}
```
