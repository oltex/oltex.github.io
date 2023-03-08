---
title: "더미 노드(Dummy Node)"
categories:
  - data structure
tags:
  - tag
---
> ## 개요

더미 노드는 연결 리스트에서 사용되는 개념입니다.

더미 노드란 유효한 데이터를 저장하지 않는 노드를 의미합니다.
아무런 값을 취하지 않는 노드를 둠으로써 리스트의 행동을 효과적으로 변형할 수 있습니다.

더미 노드는 노드이지만 연결 리스트를 유용하게 만들기 위한 노드이므로
이를 이해하기 위해서는
노드 -> 연결 리스트 -> 더미 노드 순으로 이해하는 것이 좋습니다.

연결리스트에서 생기는 문제점을 얘기하고
연결리스트에 더미노드를 적용해 보겠습니다.
> ## 문제

일단 연결 리스트의 중간 삽입 코드를 좀 정리해 보겠습니다.
```cpp
template<typename _Ty>
void List<_Ty>::Emplace(const Iterator<_Ty>& iter, const _Ty& value) {
	Node<_Ty>* cur = (*iter);
	if(nullptr == cur)
		return;
	Node<_Ty>* node = new Node<_Ty>{ value };
	Node<_Ty>* next = cur->_next;

	if (nullptr != cur)
		cur->_next = node;
	node->_prev = cur;
	node->_next = next;
	if (nullptr != next)
		next->_prev = node;
	else
		_tail = node;

	++_size;
}
```
이 코드에서 다른 문제가 존재하는데
맨앞이나 맨뒤 둘중 하나는 노드를 추가할 수 없다는 문제입니다.
현재 코드를 보면 cur와 next의 사이에 node를 추가하고 있습니다.

그렇다면 가장 앞 노드인 begin이 와도 begin의 앞에는 정작 노드를 추가할 수 없습니다.
반대로 cur과 prev로 만든다면 begin은 처리되지만 end가 오면 같은 문제가 발생합니다.
> ## 해결

이를 위해서 더미노드를 사용합니다.
리스트의 tail에 더미 노드를 둠으로써 위에서 생겼던 문제를 해결합니다.
추가로 더미 노드를 사용함에 있어 탐색에서도 유용한 기능이 생깁니다.
다만 더미 노드를 사용하려면 기존 코드를 많이 수정해야합니다.

따라서 더미 노드를 사용하여 삽입 뿐만 아니라
기존 기능들 또한 구현해 보도록 하겠습니다.
### 더미 노드
먼저 더미 노드를 만들어야합니다. 기존 노드 코드를 봐보겠습니다.
```cpp
//Node.h
template<typename _Ty>
struct Node final {
	explicit Node(const _Ty& value) :
		_value(value) {
	}
	_Ty _value;
	Node<_Ty>* _prev = nullptr;
	Node<_Ty>* _next = nullptr;
};
```
이 노드를 하나 만들어서 더미 노드로 사용할 수 있겠지만
문제는 템플릿 Ty가 클래스이고 생성자가 무조껀 매개 변수를 받게 만들어져 있다면
더미 노드 생성에 제한이 걸립니다.

따라서 널 객체(null object) 패턴을 사용하여 이를 해결해 보겠습니다.
```cpp
//Node.h
struct Node abstract {
	Node* _prev = nullptr;
	Node* _next = nullptr;
};

template<typename _Ty>
struct ListNode final : public Node {
	explicit ListNode(const _Ty& value) :
		_value(value) {
	}
	_Ty _value;
};

struct DummyNode final : public Node {
};
```
이제 노드를 생성할 때는 ListNode를
더미 노드를 생성할 때는 DummyNode를 사용하여 관리합니다.
#### 클래스
이제 더미 노드를 사용함에 맞게 클래스를 수정해야 합니다.
```cpp
//List.h
template<typename _Ty>
class List final {
private:
	Node* _head = nullptr; //이제 노드는 Node*로 저장합니다.
	Node* _tail = nullptr;
	Node* _dummy = nullptr;
	size_t _size = 0;
};
```
기존 리스트 헤더와 다른점은
head와 tail이 Node 인터페이스를 참조하게 되었습니다.
추가된 사항은 더미 노드를 참조할 dummy 맴버 변수가 생겼습니다.
### 생성
기존 코드에는 없던 부분입니다.
```cpp
//List.h
template<typename _Ty>
class List final {
public:
	explicit List(void);
};
```
```cpp
List.cpp
template<typename _Ty>
List<_Ty>::List(void) {
	_head = _tail = _dummy = new DummyNode;
}
```
이제 클래스의 생성자에서 더미 노드를 생성하여 보관해줍니다.
### 소멸
소멸자는 변경된 부분이 없습니다.
기존 코드로 남겨두겠습니다.
```cpp
//List.h
template<typename _Ty>
class List final {
public:
	~List(void);
};
```
```cpp
//List.cpp
template<typename _Ty>
List<_Ty>::~List(void) {
	while (nullptr != _head)
		Pop_Front();
}
```
### 삽입
삽입 함수입니다.
삽입 함수에는 push_front, push_back, emplace가 존재합니다.
코드 중복을 줄이기 위해 push_front와 push_back은 emplace를 호출하도록 만들었습니다.
```cpp
//List.h
template<typename _Ty>
class List final {
public:
	void Push_Front(const _Ty& value);
	void Push_Back(const _Ty& value);
	void Emplace(const Iterator<_Ty>& iter, const _Ty& value);
};
```
```cpp
//List.cpp
template<typename _Ty>
void List<_Ty>::Push_Front(const _Ty& value) {
	Emplace(Begin(), value);
}

template<typename _Ty>
void List<_Ty>::Push_Back(const _Ty& value) {
	Emplace(End(), value);
}

template<typename _Ty>
void List<_Ty>::Emplace(const Iterator<_Ty>& iter, const _Ty& value) {
	Node* cur = (*iter);
	if (nullptr == cur)
		return;
	Node* prev = cur->_prev;
	Node* node = new ListNode<_Ty>{ value };

	if (nullptr != prev)
		prev->_next = node;
	else
		_head = node;

	node->_prev = prev;
	node->_next = cur;

	cur->_prev = node;

	++_size;
}
```
push_front와 push_back에서 각각 begin과 end를 사용해 emplace를 호출하고 있습니다.
의외로 emplace함수는 변경된 사항이 거의 없습니다.

prev와 cur사이에 노드를 추가하는 코드는 그대로입니다.
이러면 begin일 때는 head의 앞에,
tail일 때는 tail의 앞에지만 tail은 더미노드이니
맨 마지막에 추가 하는 상황이 됩니다.

기존 코드와 똑같지만 tail이 dummy라는것 만으로 리스트의
맨 앞과 맨 끝에 노드를 추가할 수 있게 되었습니다.

### 삭제
삭제 함수에는 pop_front, pop_back, erase가 존재합니다.
삽입 함수와 마찬가지로 pop_front, pop_back 함수가 erase를 호출하도록 구현해보겠습니다.
```cpp
//List.h
template<typename _Ty>
class List final {
public:
	void Pop_Front(void);
	void Pop_Back(void);
	Iterator<_Ty> Erase(const Iterator<_Ty>& iter);
};
```
```cpp
//List.cpp
template<typename _Ty>
void List<_Ty>::Pop_Front(void) {
	Erase(Begin());
}

template<typename _Ty>
void List<_Ty>::Pop_Back(void) {
	Iterator<_Ty> iter = End();
	--iter;
	Erase(iter);
}

template<typename _Ty>
Iterator<_Ty> List<_Ty>::Erase(const Iterator<_Ty>& iter) {
	Node* cur = (*iter);
	if (nullptr == cur || _dummy == cur)
		return Iterator<_Ty>{ cur };

	Node* prev = cur->_prev;
	Node* next = cur->_next;

	if (nullptr != prev)
		prev->_next = next;
	else
		_head = next;

	if (nullptr != next)
		next->_prev = prev;
	else
		_tail = prev;

	delete cur;
	--_size;
	return Iterator<_Ty>{ next };
}
```
