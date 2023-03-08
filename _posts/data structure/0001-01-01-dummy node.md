---
title: "더미 노드(Dummy Node)"
categories:
  - data structure
tags:
  - tag
---
> ## 개요

더미 노드는 연결 리스트에서 사용되는 개념입니다.<br>
<br>
더미 노드란 유효한 데이터를 저장하지 않는 노드를 의미합니다.<br>
아무런 값을 취하지 않는 노드를 둠으로써 자료 구조의 행동을 효과적으로 변경할 수 있습니다.<br>
> ## 문제

어떠한 리스트가 존재하고<br>
여기에 데이터를 추가할 수 있는 함수가 존재한다고 가정해보겠습니다.
```cpp
struct Node final {
	Node(int data) :
		_data(data) {
	}
	int _data = 0;
	Node* _next = nullptr;
};
```
```cpp
class List final {
public:
	void Push(int data) {
		Node* node = new Node(data);
		if (nullptr == _head)
			_head = node;
		else
			_tail->_next = node;

		_tail = node;
	}
private:
	Node* _head = nullptr;
	Node* _tail = nullptr;
};
```
```cpp
void main(void) {
	List list;
	list.Push(10);
	list.Push(20);
	list.Push(30);
}
```
이 코드에서 한가지 확인 해야 할 점은<br>
Push 함수에서 노드가 존재할 때랑 존재하지 않을 때의 행동이 다르다는 것입니다.<br>
<br>
앞으로 리스트의 함수를 만들 때 <br>
존재하는지 안하는지에 따른 예외 처리를 해줘야하는 불편함이 생기게 됩니다.
> ## 해결

이러한 코드를 간략화 시키기 위해 더미 노드를 사용해 보겠습니다.
```cpp
class List final {
public:
	List(void) {
		_head = _tail = new Node(-1);
	}
	void Push(int data) {
		Node* node = new Node(data);
		_tail->_next = node;
		_tail = node;
	}
private:
	Node* _head = nullptr;
	Node* _tail = nullptr;
};
```
클래스의 생성자에 더미 노드를 추가하였습니다.<br>
이후 Push 함수에서 더는 노드가 존재하는지 확인할 필요가 사라졌습니다.<br>
<br>
결과적으로 코드를 간략하게 만들 수 있게 되었습니다.
> ## 활용

이처럼 더미 노드는 노드를 사용하는 자료 구조의 효과적인 활용을 위해<br>
만들어졌습니다.<br>
<br>
여기서는 삽입 함수 하나에서만 적용된 결과를 보았지만<br>
탐색이나 중간 삽입, 삭제 함수에서도 유용하게 사용됩니다.
