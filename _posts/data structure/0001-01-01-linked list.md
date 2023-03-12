---
title: "연결 리스트(Linked List)"
categories:
  - data structure
tags:
  - tag
---
> ## 개요

선형 자료구조로 노드를 연결하여 하나의 연결된 자료 구조를 구성합니다.<br>
<br>
어떤 데이터를 저장할 때, 다른 데이터의 위치를 포함시키는 방법으로 자료를 저장하는 방법입니다.<br>
노드에 다른 노드를 가리키는 포인터를 둔 후 노드끼리 연결 고리를 만듭니다.<br>
<br>
배열에 비해 삽입 삭제가 용이하고 빠릅니다.<br>
허나 탐색에서는 리스트는 임의 접근이 불가능해 불리합니다.<br>
<br>
배열에 비해 자료 구조의 크기가 유동적으로 변할 수 있다는 장점을 가지고있지만<br>
포인터에 드는 비용이 추가적으로 들고, 데이터 지역성이 떨어져 캐시 적중률이 낮습니다.
> ## 구조

연결 리스트를 이루는 구조는 다음과 같습니다.
- Node: 연결 리스트에서 관리할 데이터의 덩어리 입니다. 노드는 다음 노드에 대한 정보를 갖습니다.
- List: 연결 리스트를 구성할 클래스입니다. 첫번째 노드를 참조하는 head 포인터를 갖습니다.

디음은 연결 리스트 클래스인 list가 갖는 함수들입니다.
- Push: 노드를 추가합니다. 노드의 가장 앞 부분에 추가됩니다.
- pop: 노드를 제거합니다. 노드의 가장 앞 부분을 제거합니다.

> ## 구현

노드라는 구조체와 리스트라는 클래스를 사용하여 만든<br>
연결 리스트의 기본적인 구조입니다.
### 노드
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
노드 구조체 입니다.<br>
<br>
다음 노드에 대한 정보를 가지는 next 포인터가 존재합니다.<br>
노드의 데이터에 대한 변수인 value가 존재하고.<br>
노드를 생성할 때 생성자에서 value를 초기화 합니다.
### 리스트
```cpp
template<typename _Ty>
class List final {
public:
	void Push(_Ty value) {
		Node<_Ty>* node = new Node<_Ty>{ value };

		node->_next = _head;
		_head = node;
	}
	void Pop(void) {
		if (nullptr == _head)
			return;

		Node<_Ty>* head = _head;
		_head = _head->_next;
		delete head;
	}
private:
	Node<_Ty>* _head = nullptr;
};
```
리스트 클래스 입니다.<br>
<br>
노드의 가장 앞 노드를 참조할 수 있는 head 변수가 존재합니다.<br>
노드를 삽입할 수 있는 Push 함수가 존재합니다. 노드를 생성하고<br>
기존 head를 node의 next로 만든 후, node를 head로 만듭니다.<br>
<br>
노드를 삭제할 수 있는 Pop 함수가 존재합니다.<br>
head 를 head의 next로 설정하고 이전 head를 삭제합니다.
> ## 종류

연결 리스트는 위처럼 단순한 구조로 작성 되지만은 않습니다.<br>
좀더 세분화 되고 추가된 여러 형태의 연결 리스트들이 존재합니다.<br>
<br>
연결 리스트의 대표적인 종류는 다음과 같습니다.
- 단일 연결 리스트(Singly linked list)<br>
노드의 연결이 단방향으로 이루어져 있습니다. 노드는 다음 노드의 참조를 가집니다.<br>
head노드를 사용합니다.마지막 원소를 가리키는 tail을 가지는 형태도 존재합니다.<br>
보통 큐를 구현하는데 사용합니다.
- 이중 연결 리스트(Doubly linked list)<br>
노드의 연결이 양방향으로 이루어져 있습니다. 노드는 이전/다음 노드의 참조를 가집니다.<br>
head와 tail노드를 사용합니다. 보통 리스트를 구현하는데 사용합니다.<br>
노드의 삭제에 드는 비용이 단순 연결 리스트는 O(n)이고 이중 연결 리스트는 O(1)로 적게듭니다.
- 원형 연결 리스트(circular linked list)<br>
노드의 연결이 원형으로 이루어져 있습니다. 마지막 노드의 다음 노드가 첫번째 노드를 참조하는 방식입니다.<br>
스트림 버퍼의 구현에 사용됩니다.
