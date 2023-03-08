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
> ## 문제

일단 연결 리스트의 중간 삽입 코드를 좀 정리해 보겠습니다.
```cpp
template<typename _Ty>
void List<_Ty>::Emplace(const Iterator<_Ty>& iter, const _Ty& value) {
	Node<_Ty>* node = new Node<_Ty>{ value };

	Node<_Ty>* cur = (*iter);
	Node<_Ty>* next = cur->_next;

	if (nullptr != cur)
		cur->_next = node;
	node->_prev = cur;
	node->_next = next;
	if (nullptr != next)
		next->_prev = node;

	++_size;
}
```
여기서 head와 tail에 대한 처리가 없다고 했었는데 이는 잠시 미뤄두겠습니다.

이 코드에서 다른 문제가 존재하는데
맨앞이나 맨뒤 둘중 하나는 노드를 추가할 수 없다는 문제입니다.
현재 코드를 보면 cur와 next의 사이에 node를 추가하고 있습니다.

그렇다면 가장 앞 노드인 begin이 와도 begin의 앞에는 정작 노드를 추가할 수 없습니다.
반대로 cur과 prev로 만든다면 begin은 처리되지만 end가 오면 같은 문제가 발생합니다.

> ## 해결

이를 위해서 더미노드를 사용합니다.
더미 노드에 대한 설명은 개요에 존재하니 바로 구현해 보도록 하겠습니다.

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

struct NullNode final : public Node {
};
```
이제 노드를 생성할 때는 ListNode를
더미 노드를 생성할 때는 NullNode를 사용하여 관리합니다.

#### 기존 코드 수정

이를 더미 노드 기반 연결 리스트라고 부르는데
더미 노드를 사용함에 맞게 리스트와 반복자 클래스를 수정해야 합니다.

먼저 더미 노드보다 이들의 인터페이스에 맞게 Node를 전부 변경하는 작업부터 하겠습니다.
```cpp
//List.h
template<typename _Ty>
class List final {
public:
	explicit List(void);
	~List(void);
public:
	void Push_Front(const _Ty& value);
	void Push_Back(const _Ty& value);

	void Pop_Front(void);
	void Pop_Back(void);

	void Emplace(const Iterator<_Ty>& iter, const _Ty& value);
	Iterator<_Ty> Erase(const Iterator<_Ty>& iter);
public:
	Iterator<_Ty> Begin(void);
	Iterator<_Ty> End(void);
private:
	Node* _head = nullptr; //이제 노드는 Node*로 저장합니다.
	Node* _tail = nullptr;
	size_t _size = 0;
};
```
