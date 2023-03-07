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
	Node<T>* _next = nullptr;
};
```
```cpp
//LinkedList.h

template<typename T>
class LinkedList final {
public:
	LinkedList(void);
	~LinkedList(void);
private:
	Node<T>* _head = nullptr;
	Node<T>* _tail = nullptr;
};
```
```cpp
//LinkedList.cpp

template<typename T>
LinkedList<T>::LinkedList(void) {
}

template<typename T>
LinkedList<T>::~LinkedList(void) {
}
```

### 삽입

이제 틀을 만들었으니 여기에 들어갈 기능을 구현해야합니다.
가장 먼저 노드의 삽입을 구현해보겠습니다.

가장 먼저 노드를 생성할 때 value를 받을 수 있게
노드의 생성자를 구현하였습니다.
```cpp
//Node.h
template<typename T>
struct Node {
	Node(T& value) :
		_value(value) {
	}
};
```

노드의 삽입을 위해 push_front와 push_back 함수를 만들었습니다.

