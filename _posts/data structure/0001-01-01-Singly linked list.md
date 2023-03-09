---
title: "단순 연결 리스트(Singly Linked List)"
categories:
  - data structure
tags:
  - tag
---
> ## 구현

단순 연결 리스트를 구현해보도록 하겠습니다.

각 연결 리스트들은 내용이 상당히 중복 되기 때문에
가장 대표적인 이중 연결 리스트를 먼저 구현하였습니다.

따라서 단순 연결 리스트는 설명보다는 구현에 초점을 두고
이중 연결 리스트와의 차이점에 대해서 만 자세히 설명하겠습니다.

### 구조체
가장 먼저 노드 구조체를 구현해 보도록 하겠습니다.
```cpp
struct Node abstract {
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
Node는 다음 노드를 참조할 next 포인터를 가지고있습니다.
ListNode는 실제 노드를 담당합니다. 구체화된 노드의 구조체 입니다.
DummyNode는 더미 노드로, 널 객체 패턴을 사용하여 구현하였습니다.

### 클래스
단순 연결 리스트의를 클래스를 구현해보겠습니다.
정의부를 먼저 구현한 다음 기능에 맞춰 구현부를 작성하겠습니다.
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
	void Pop_Front(void);

	Iterator Emplace(const Iterator& iter, const _Ty& value);
	Iterator Erase(const Iterator& iter);

	Iterator Begin(void);

	size_t Size(void);
private:
	Node* _head = nullptr;
	size_t _size = 0;
};
```
리스트 클래스 안에는 가장 앞 노드를 기리킬 head 포인터가 존재합니다.
그리고 리스트의 크기를 저장할 size변수 가 존재합니다.

### 크기
리스트의 크기를 반환할 사이즈 함수입니다.
```cpp
template<typename _Ty>
size_t List<_Ty>::Size(void) {
	return _size;
}
```
이를 위해 리스트의 삽입/삭제 함수의 끝에 size의 증감 연산을 추가합니다.

### 삽입
단순 연결 리스트에서 삽입이 이뤄지는 경우는 크게 3가지가 존재합니다.
1. 리스트의 생성시 헤드 노드로 더미 노드를 삽입합니다.
2. 리스트의 가장 앞에 노드를 삽입합니다.
3. 리스트의 중간 부분에 노드를 삽입할 수 있어야 합니다.

여기서 3번째는 선택 사항입니다.
STL의 단순 연결 리스트인 forward_list가 이를 지원하기 때문에 작성하였습니다.

1.
먼저 생성자를 구현해보겠습니다.
```cpp
template<typename _Ty>
List<_Ty>::List(void) :
	_head(new DummyNode) {
}
```
생성자에서 리스트의 head 맴버 변수에 더미 노드를 미리 추가해 둡니다.

2.
다음은 리스트의 가장 앞에 노드를 추가해야 합니다.
이를 위해 push_front 함수를 구현하였습니다.
```cpp
template<typename _Ty>
void List<_Ty>::Push_Front(const _Ty& value) {
	Emplace(Begin(), value);
}
```
코드 중복을 줄이기 위해 중간 노드 삽입 함수인 emplace를 재사용 했습니다.
여기서 들어가는 함수인 begin이 노드의 가장 앞을 가리킵니다.

3.
이제 중간에 들어가는 함수를 작업해야 합니다.
함수의 이름은 emplace로 매개 변수로 반복자와 값을 전달 받습니다.
```cpp
template<typename _Ty>
Iterator<_Ty> List<_Ty>::Emplace(const Iterator& iter, const _Ty& value) {
	Node* prev = iter.Prev();
	if (nullptr == prev)
		return Iterator{};
	Node* node = new ListNode<_Ty>{ value };
	Node* cur = (*iter);

	prev->_next = node;
	node->_next = cur;

	++_size;

	return Iterator{ node, cur };
}
```
함수의 삽입을 위해서 어쩔 수 없이 이전 노드인 prev와 cur을 받습니다.
그리고 이 사이에 node를 끼워 넣습니다.

이 과정에서 사용된 반복자는 손상되기 때문에
모든 작업이 끝나면 node와 cur를 참조하는 반복자를 반환하게 .

### 삭제
다음은 삭제 함수를 작성해야합니다.
단순 연결 리스트의 삭제에는 다음과 같은 경우가 존재합니다.
1. 가장 앞 노드를 삭제할 수 있어야 합니다.
2. 중간에 위치한 노드를 삭제할 수 있어야 합니다.
3. 리스트가 소멸할 때 모든 노드를 삭제해야 합니다.

여기도 마찬가지로 2번째 과정은 구현하지 않아도 됩니다.
STL 단순 연결 리스트인 forward_list는 이를 지원합니다.

1.
가장 앞 노드를 삭제하는 함수의 이름은 pop_front입니다.
```cpp
template<typename _Ty>
void List<_Ty>::Pop_Front(void) {
	Erase(Begin());
}
```
이 함수 또한 코드의 재사용을 높이기 위해 중간 삭제 함수인 erase를 호출하게 만들었습니다.
여기서 들어가는 함수인 begin이 노드의 가장 앞을 가리킵니다.

2.
중간 삭제 함수를 구현해야 합니다.
중간 삭제 함수의 이름은 erase입니다. 매개 변수로 반복자를 받습니다.
```cpp
template<typename _Ty>
Iterator<_Ty> List<_Ty>::Erase(const Iterator& iter) {
	Node* cur = (*iter);
	if (nullptr == cur || _head == cur)
		return Iterator{};
	Node* prev = iter.Prev();
	Node* next = cur->_next;

	prev->_next = next;

	delete cur;
	--_size;
	return Iterator{ prev, next };
}
```
반복자를 사용하여 이전 노드 prev와 다음 노드 next를 가져옵니다.
이후 cur 노드를 삭제한 후 prev노드와 next 노드를 연결합니다.

이 과정에서 사용되었던 반복자는 손상되기 때문에
새로운 반복자를 반환하게 만듭니다.

3.
마지막으로 소멸자 입니다.
```cpp
```
소멸시 head는 더미 노드이고 head의 다음 노드가 존재하지 않을 때까지 반복문을 돌립니다.
