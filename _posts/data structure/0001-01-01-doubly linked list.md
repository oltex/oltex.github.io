---
title: "이중 연결 리스트(Doubly Linked List)"
categories:
  - data structure
tags:
  - tag
---
> ## 개요


> ## 구현

연결 리스트를 구현해보도록 하겠습니다.<br>
<br>
제 생각으로 구현한 코드이기 때문에 많이 부족합니다.<br>
너그러운 마음으로 봐주시면 감사하겠습니다.<br>
(잘못된점은 지적해주시면 감사하겠습니다.)

---
### 구조체
먼저 가장 기본이 되는 node구조체를 구현해보겠습니다.
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
Node라는 인터페이스를 생성하였습니다.<br>
이전/다음 노드를 가리킬 prev, next 포인터가 존재합니다.<br>
 <br>
ListNode 구조체안에는 값을 저장할 value 변수가 존재합니다.<br>
노드를 생성할 때 값을 받을 수 있게 ListNode의 생성자를 구현하였습니다.<br>
노드는 생성자를 통해 value를 받아 자신의 구조체 변수 안에 저장합니다.<br>
<br>
DummyNode 구조체는 널 객체(null object) 패턴을 사용하여 만들어진 구조체입니다.<br>
ListNode노드를 사용하여 더미 노드로 사용하려 한다면<br>
템플릿 Ty가 클래스이고 생성자가 무조껀 매개 변수를 받게 만들어져 있다면<br>
더미 노드 생성에 제한이 걸립니다.<br>
따라서 널 객체 패턴을 사용하여 이를 해결하였습니다.

---
### 클래스
그다음 노드 구조체를 관리하는 List 클래스를 구현해보겠습니다.<br>
List 클래스는 다양한 타입을 받기 위해 템플릿을 사용하였습니다.
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

	Iterator<_Ty> Begin(void);
	Iterator<_Ty> End(void);
  
	size_t Size(void);
private:
	Node* _head = nullptr;
	Node* _tail = nullptr;
	Node* _dummy = nullptr;
	size_t _size = 0;
};
```
리스트 클래스안에는 가장 앞 노드를 가리킬 head 포인터와<br>
가장 뒷 노드를 가리킬 tail 포인터가 존재합니다.<br>
그리고 더미 노드를 판단하기 위한 dummy 포인터가 존재합니다.<br>
마지막으로 리스트의 크기를 저장할 size 변수가 존재합니다.<br>
<br>
나머지 함수 들은 이후에 설명하겠습니다.

---
### 크기
배열은 자신이 가지고 있는 크기가 고정적이지만 리스트는 크기가 유동적으로 변합니다.<br>
이에 리스트의 원소들을 탐색하지 않고 리스트의 사이즈만 알고 싶은 경우가 존재할 것입니다.<br>
<br>
이러한 상황에서 매번 리스트를 돌며 갯수를 세는 것은 불필요한 자원 낭비일 것입니다.<br>
따라서 size라는 변수를 추가하여 크기를 저장할 수 있습니다.
```cpp
template<typename _Ty>
size_t List<_Ty>::Size(void) {
	return _size;
}
```
이제 삽입/삭제의 함수 끝에 증감 연산자를 사용해줍니다.

---
### 삽입
이제 틀을 만들었으니 여기에 들어갈 기능을 구현해야합니다.<br>
<br>
가장 먼저 노드의 삽입을 구현해보겠습니다.<br>
노드의 삽입은 다음과 같은 경우가 존재합니다.
1. 객체가 생성될 때 더미 노드를 삽입해야 합니다.
2. 노드의 머리와 꼬리 부분에 삽입할 수 있어야 합니다.
3. 노드의 중간 위치에 삽입할 수 있어야 합니다.

먼저 생성자를 구현해보겠습니다. 
```cpp
template<typename _Ty>
List<_Ty>::List(void) {
	_head = _tail = _dummy = new DummyNode;
}
```
생성자에서 할당된 더미 노드를 3가지 포인터에 전부 넣어줍니다.<br>
<br>
다음은 노드의 삽입을 구현해야 합니다.<br>
이를 위해 push_front와 push_back 내부를 구현해보겠습니다.
```cpp
template<typename _Ty>
void List<_Ty>::Push_Front(const _Ty& value) {
	Emplace(Begin(), value);
}

template<typename _Ty>
void List<_Ty>::Push_Back(const _Ty& value) {
	Emplace(End(), value);
}
```
코드 중복을 줄이기 위해 push_front와 push_back에서<br>
각각 begin과 end를 사용해 emplace를 호출하게 만들었습니다.<br>
<br>
begin과 end는 각각 처음 노드와 마지막 노드에 대한 반복자를 호출한다고 생각하면 되겠습니다.<br>
반복자는 탐색에서 설명하겠습니다.<br>
<br>
리스트를 사용하다 보면 중간에 있는 값을 삽입하고 싶은 경우가 존재합니다.<br>
이를 위해 emplace 함수를 구현해 보겠습니다.
```cpp
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
중간이라는 지점에 접근하기 위해서는 반복자를 사용해야 합니다.<br>
허나 반복자 내에서 삭제가 이뤄지면 미리 만들어둔 size에 영향이 갑니다.<br>
게다가 삽입과 삭제를 담당해야 하는 것은 리스트 클래스 입니다.<br>
<br>
따라서 반복자를 매개변수로 받는 리스트 맴버함수를 제작해야합니다.<br>
<br>
iter가 가리키는 cur노드와 이전 prev노드 사이에 새로운 node를 끼워넣습니다.<br>
이를 수행하기 위해 각 연결고리를 수정해주고 있습니다.<br>

여기서 더미 노드를 사용할 때의 이점이 드러납니다.
- begin일 때는 head의 앞에 추가하여 가장 앞에 노드를 추가합니다.
- end일 때는 tail의 앞에 추가하지만 tail은 더미 노드이니맨 마지막에 추가 하는 상황이 됩니다.

tail이 dummy라는것 만으로<br>
하나의 함수로 리스트의 맨 앞과 맨 끝 양쪽에 노드를 추가하는 기능을 만들 수 있습니다.

---
### 삭제
노드를 삽입했으면 이를 삭제하는 방법도 존재해야 합니다.<br>
삭제에는 3가지 경우가 있습니다.
1. 노드의 머리와 꼬리부분을 삭제할 수 있어야 하고
2. 노드의 중간 부분을 삭제할 수 있어야 합니다.
3. 클래스를 삭제할 때는 모든 노드를 삭제하여야 합니다.

노드의 삭제를 위해 pop_front, pop_back, erase 함수 내부를 구현해보겠습니다.
```cpp
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
```
마찬가지로 삭제 함수 또한 코드중복을 줄였습니다.<br>
대신 pop_back에서 End는 더미 노드를 반환하니 이전 노드를 참조하기 위해 증감 연산자를 사용합니다.
```cpp
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

	next->_prev = prev;

	delete cur;
	--_size;
	return Iterator<_Ty>{ next };
}
```
삭제 함수에서는 반복자가 가리키는 노드 cur를 삭제합니다.<br>
그 과정에서 cur의 이전 노드 prev와 다음 노드 next를 연결합니다.<br>
<br>
만약 prev가 nullptr이라는 것은 cur가 head였다는 뜻이 되니<br>
head를 next로 설정해 줍니다.<br>
<br>
erase를 살펴보면 cur가 dummy 노드라면 리턴하는 코드가 존재합니다.<br>
따라서 tail이 들어오는 경우는 없으니 next는 무조껀 존재합니다.<br>
<br>
리스트 클래스가 소멸할 때 노드를 같이 해제시켜줘야 합니다.<br>
리스트 클래스의 소멸자를 구현해 보겠습니다.
```cpp
//List.cpp
template<typename _Ty>
List<_Ty>::~List(void) {
	while (nullptr != _head)
		Pop_Front();
}
```
간단한 코드입니다. 기존 pop_front를 이용하여<br>
head가 nullptr이 될때까지 모든 노드를 삭제합니다.

---
### 탐색
이제 노드의 삽입과 삭제 기능이 구현 되었습니다.<br>
이 자료 구조를 사용하기 위해서는 저장된 노드를 탐색하는 기능이 필요할 것입니다.<br>
<br>
리스트에 이를 구현해도 좋지만<br>
자료 구조의 탐색을 분리하기 위해 반복자 패턴을 사용하겠습니다.<br>
<br>
이를 위해 Iterator클래스를 구현해보겠습니다.<br>
(Iterator클래스는 인터페이스 클래스로 만들지만 여기서는<br>
list하나만을 이야기하기 때문에 구체 클래스로 제작하겠습니다.)
```cpp
template<typename _Ty>
class Iterator final {
public:
	explicit Iterator(Node* cur);
public:
	ListNode<_Ty>* operator*(void) const;

	void operator++(void);
	void operator--(void);

	bool operator!=(const Iterator<_Ty>& rhs);
private:
	Node* _cur = nullptr;
};
```
반복자 클래스의 생성자는 본인이 탐색을 시작할 노드를 받아 맴버 변수로 저장합니다.<br>
이 노드 맴버 변수는 리스트의 head 또는 tail을 받게됩니다.<br>
```cpp
template<typename _Ty>
Iterator<_Ty>::Iterator(Node<_Ty>* cur) :
	_cur(cur) {
}
```
매번 반복자를 생성할 때마다 리스트안의 노드의 head와 tail을 넘겨줄 순 없으니<br>
반복자 클래스를 생성하는 팩토리 메서드를 리스트 클래스에 구현해 줍니다.
```cpp
template<typename _Ty>
Iterator<_Ty> List<_Ty>::Begin(void) {
	return Iterator<_Ty>{ _head };
}

template<typename _Ty>
Iterator<_Ty> List<_Ty>::End(void) {
	return Iterator<_Ty>{ _tail };
}
```
begin 함수는 리스트 클래스에 있는 반복자 팩토리 메서드 입니다.<br>
자신의 head를 반복자의 생성자에 넘겨주어 반복자를 생성하고 이를 반환합니다.<br>
<br>
이제 반복자가 생성되었으니 반복자를 통해 node를 받아올 수 있게 만들어야 합니다.
```cpp
template<typename _Ty>
ListNode<_Ty>* Iterator<_Ty>::operator*(void) const {
	return static_cast<ListNode<_Ty>*>(_cur);
}
```
반복자의 cur가 가리키는 node를 반환합니다.<br>
이 때 Node 추상 구조체(?)의 구체화를 위해 dynamic_cast를 사용했습니다.<br>
반복자가 node를 반환할 수 있게 되었습니다.<br>
<br>
마지막으로 반복자라는 이름에 걸맞게 노드를 반복 즉 순회할 수 있어야 합니다.
```cpp
template<typename _Ty>
void Iterator<_Ty>::operator++(void) {
 	_cur = _cur->_next;
}

template<typename _Ty>
void Iterator<_Ty>::operator--(void) {
	_cur = _cur->_prev;
}

template<typename _Ty>
bool Iterator<_Ty>::operator!=(const Iterator<_Ty>& rhs) {
	if (_cur != rhs._cur)
		return true;
	return false;
}
```
증감 연산자 오버로드는 cur의 next 또는 prev를 cur에 대입하게 하여 노드를 순회합니다.<br>
<br>
비교 연산자의 오버로드는 반복자를 비교하여<br>
반복자가 가리키는 노드가 다르면 true 같으면 false을 보냅니다.<br>
<br>
이 두가지를 사용하여 반복자의 탐색을 구현할 수 있습니다.

---

> ## 사용

이제 구현한 이중 연결 리스트를 메인 함수에서 사용해 보겠습니다.
```cpp
void main(void) {
	List<int> list;

	list.Push_Back(10);
	list.Push_Back(20);
	list.Push_Back(30);
	list.Push_Back(40);
	list.Push_Back(50);

	list.Pop_Back();
	list.Pop_Front();

	Iterator<int> iter = list.Begin();

	iter = list.Begin();
	for (int i = 0; i < list.Size(); ++i) {
		std::cout << (*iter)->_value << std::endl;
		++iter;
	}

	++iter;
	++iter;
	list.Emplace(iter, 1000);
	list.Erase(iter);

	for (Iterator<int> iter = list.Begin(); iter != list.End(); ++iter) {
		std::cout << (*iter)->_value << std::endl;
	}

	list.~List();
};

```
