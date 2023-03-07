---
title: "연결 리스트(Linked List)"
categories:
  - data structure
tags:
  - tag
---
> ## 개요

노드를 연결하여 하나의 리스트를 구성합니다.

> ## 종류

이 글에서는 양방향 연결 리스트로 구현하겠습니다.

> ## 구현

연결 리스트를 구현해보도록 하겠습니다.
구현을 위해 클래스와 템플릿을 사용하였습니다.

제 생각으로 구현한 코드이기 때문에 많이 부족합니다.
너그럽게 봐주시면 감사하겠습니다.
(지적해주시면 감사하겠습니다.)

### 기본
먼저 가장 기본이 되는
node구조체와 linkedlist클래스를 구현해보겠습니다.
```cpp
//Node.h
template<typename T>
struct Node {
	T _value = 0;
	Node<T>* _prev = nullptr;
	Node<T>* _next = nullptr;
};
```
```cpp
//LinkedList.h
template<typename T>
class LinkedList final {
private:
	Node<T>* _head = nullptr;
	Node<T>* _tail = nullptr;
};
```

### 삽입

이제 틀을 만들었으니 여기에 들어갈 기능을 구현해야합니다.
가장 먼저 노드의 삽입을 구현해보겠습니다.

가장 먼저 노드를 생성할 때 value를 받을 수 있게 노드의 생성자를 구현하였습니다.
```cpp
//Node.h
template<typename T>
struct Node {
	Node(T& value) :
		_value(value) {
	}
};
```

이제 클래스에 노드의 삽입을 위해 push_front와 push_back 함수를 만들었습니다.
```cpp
//LinkedList.h
template<typename T>
class LinkedList final {
public:
	void Push_Front(const T& value);
	void Push_Back(const T& value);
};
```
```cpp
//LinkedList.cpp
template<typename T>
void LinkedList<T>::Push_Front(const T& value) {
	Node<T>* node = new Node<T>{ value };
	if (nullptr == _tail)
		_tail = node;
	else {
		_head->_prev = node;
		node->_next = _head;
	}
	_head = node;
}

template<typename T>
void LinkedList<T>::Push_Back(const T& value) {
	Node<T>* node = new Node<T>{ value };
	if (nullptr == _head)
		_head = node;
	else {
		_tail->_next = node;
		node->_prev = _tail;
	}
	_tail = node;
}
```

### 삭제

노드를 삽입했으면 이를 삭제하는 방법도 존재해야 합니다.
삭제에는 2가지 경우가 있습니다.

1. 노드의 머리와 꼬리부분을 삭제할 수 있어야 하고
2. 클래스를 삭제할 때는 모든 노드를 삭제하여야 합니다.

노드의 삭제를 위해 pop_front와 pop_back 함수를 만들어보겠습니다.
```cpp
//LinkedList.h
template<typename T>
class LinkedList final {
public:
	void Pop_Front(void);
	void Pop_Back(void);
};
```
```cpp
//LinkedList.cpp
template<typename T>
void LinkedList<T>::Pop_Front(void) {
	if (nullptr == _head)
		return;

	Node<T>* node = _head->_next;
	if (nullptr != node)
		node->_prev = nullptr;
	else
		_tail = nullptr;

	delete _head;
	_head = node;
}

template<typename T>
void LinkedList<T>::Pop_Back(void) {
	if (nullptr == _tail)
		return;

	Node<T>* node = _tail->_prev;
	if (nullptr != node)
		node->_next = nullptr;
	else
		_head = nullptr;

	delete _tail;
	_tail = node;
}
```

이번에는 클래스가 소멸할 때 노드를 같이 해제시켜줘야 합니다.
클래스의 소멸자를 구현해 보겠습니다.
```cpp
//LinkedList.h
template<typename T>
class LinkedList final {
public:
	~LinkedList(void);
};
```
