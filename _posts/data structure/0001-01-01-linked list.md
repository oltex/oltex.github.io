---
title: "연결 리스트(Linked List)"
categories:
  - data structure
tags:
  - tag
---
> ## 개요

노드를 연결하여 하나의 리스트를 구성합니다.

> ## 기본

연결 리스트의 기본적인 구조를 구현해보도록 하겠습니다.
단방향 연결 리스트로 구현하겠습니다.

클래스와 템플릿을 사용하였습니다.
```cpp
//Node.h

template<typename T>
struct Node {
	T _data = 0;
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

> ## 푸쉬
