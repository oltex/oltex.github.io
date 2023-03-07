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

연결 리스트를 구현해보도록 하겠습니다.<br>
<br>
제 생각으로 구현한 코드이기 때문에 많이 부족합니다.<br>
너그러운 마음으로 봐주시면 감사하겠습니다.<br>
(잘못된점은 지적해주시면 감사하겠습니다.)

---
### 기본
먼저 가장 기본이 되는 node구조체를 구현해보겠습니다.
```cpp
//Node.h
template<typename T>
struct Node {
	T _value;
	Node<T>* _prev = nullptr;
	Node<T>* _next = nullptr;
};
```
노드 구조체안에는 값을 저장할 value 변수와<br>
이전/다음 노드를 가리킬 prev, next 포인터가 존재합니다.<br>
<br>
그다음 노드 구조체를 관리하는 List 클래스를 구현해보겠습니다.<br>
List 클래스는 다양한 타입을 받기 위해 템플릿을 사용하였습니다.
```cpp
//List.h
template<typename T>
class List final {
private:
	Node<T>* _head = nullptr;
	Node<T>* _tail = nullptr;
};
```
리스트 클래스안에는 가장 앞 노드를 가리킬 head 포인터와<br>
가장 뒷 노드를 가리킬 tail 포인터가 존재합니다.

---
### 삽입
이제 틀을 만들었으니 여기에 들어갈 기능을 구현해야합니다.<br>
<br>
가장 먼저 노드의 삽입을 구현해보겠습니다.<br>
노드를 생성할 때 값을 받을 수 있게 노드의 생성자를 구현하였습니다.
```cpp
//Node.h
template<typename T>
struct Node {
	explicit Node(T& value) :
		_value(value) {
	}
};
```
노드는 생성자를 통해 value를 받아 자신의 구조체 변수 안에 저장합니다.<br>
<br>
이제 리스트 클래스는 노드의 삽입연산을 지원해야 합니다.<br>
이를 위해 push_front와 push_back 함수를 만들었습니다.
```cpp
//List.h
template<typename T>
class List final {
public:
	void Push_Front(const T& value);
	void Push_Back(const T& value);
};
```
```cpp
//List.cpp
template<typename T>
void List<T>::Push_Front(const T& value) {
	Node<T>* node = new Node<T>{ value }; //1
	if (nullptr == _head) //2
		_tail = node;
	else { //3
		_head->_prev = node;
		node->_next = _head;
	}
	_head = node; //4
}

template<typename T>
void List<T>::Push_Back(const T& value) {
	Node<T>* node = new Node<T>{ value };
	if (nullptr == _tail)
		_head = node;
	else {
		_tail->_next = node;
		node->_prev = _tail;
	}
	_tail = node;
}
```
push_front 함수는 value를 받아 새 노드를 만들고, 기존 노드의 가장 앞 즉 head에 추가하는 방법입니다.<br>
설명에 맞춰 번호를 달아두었습니다.
1. 먼저 받은 value로 새로운 node를 생성하고
2. 만약 head가 nullptr이라면(기존 노드가 비었다면) tail에 node를 대입합니다.
3. 만약 head가 nullptr이 아니라면(기존 노드가 존재한다면)
head의 prev에 node를 대입해 주고<br>
node의 next에 head를 대입해 줍니다.(head와 node를 연결하기)
4. 무슨 경우든 node는 head가 됩니다.

이와 비슷한 과정을 push_back도 반복합니다.

---
### 삭제
노드를 삽입했으면 이를 삭제하는 방법도 존재해야 합니다.<br>
삭제에는 2가지 경우가 있습니다.<br>
1. 노드의 머리와 꼬리부분을 삭제할 수 있어야 하고
2. 클래스를 삭제할 때는 모든 노드를 삭제하여야 합니다.

노드의 삭제를 위해 리스트 클래스에 pop_front와 pop_back 함수를 만들어보겠습니다.
```cpp
//List.h
template<typename T>
class List final {
public:
	void Pop_Front(void);
	void Pop_Back(void);
};
```
```cpp
//List.cpp
template<typename T>
void List<T>::Pop_Front(void) {
	if (nullptr == _head) //1
		return;

	Node<T>* node = _head->_next; //2
	if (nullptr != node) //3
		node->_prev = nullptr;
	else //4
		_tail = nullptr;

	delete _head; //5
	_head = node; 
}

template<typename T>
void List<T>::Pop_Back(void) {
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
pop_front함수는 리스트의 가장 뒷 부분을 즉, head를 제거하는 함수입니다.<br>
설명에 맞춰 번호를 달아두었습니다.
1. 먼저 head가 nullptr이 아닌지 검사합니다.<br>
만약 nullptr이라면 기존 노드가 없다는 뜻이니 바로 리턴합니다.
2. head가 nullptr이 아니라면 head의 next를 node라는 변수에 받습니다.
3. 만약 node가 nullptr가 아니라면 node의 prev를 nullptr로 만듭니다.(head와 node의 연결을 끊습니다.)
4. 만약 node가 nullptr이라면(기존 노드가 딱 1개 있었다면)<br> tail을 nullptr로 만듭니다.
5. head를 소멸시키고 head에 node를 대입합니다.

이와 비슷한 과정을 pop_back도 반복합니다.<br>
<br>
이번에는 리스트 클래스가 소멸할 때 노드를 같이 해제시켜줘야 합니다.<br>
리스트 클래스의 소멸자를 구현해 보겠습니다.
```cpp
//List.h
template<typename T>
class List final {
public:
	~List(void);
};
```
```cpp
//List.cpp
template<typename T>
List<T>::~List(void) {
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
이를 위해 Iterator클래스를 구현해보겠습니다.
```cpp
//Iterator.h
template<typename _Ty>
class Iterator final {
public:
	explicit Iterator(Node<_Ty>* cur);
private:
	Node<_Ty>* _cur = nullptr;
};
```
```cpp
//Iterator.cpp
template<typename _Ty>
Iterator<_Ty>::Iterator(Node<_Ty>* cur) :
	_cur(cur) {
}

```
반복자 클래스의 생성자는 본인이 탐색을 시작할 노드를 받아 맴버 변수로 저장합니다.<br>
이 노드 맴버 변수는 리스트의 헤드 또는 테일을 받게됩니다.<br>
<br>
매번 반복자를 생성할 때마다 리스트안의 노드의 헤드와 테일을 넘겨줄 순 없으니<br>
반복자 클래스를 생성하는 팩토리 메서드를 리스트 클래스에 추가해 줍니다.
```cpp
//List.h
template<typename _Ty>
class List final {
public:
	Iterator<_Ty> Begin(void);
	Iterator<_Ty> End(void);
};
```
```cpp
//List.cpp
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
이제 반복자가 생성되었으니 반복자를 통해<br>
node를 받아올 수 있게 만들어야 합니다.<br>
이를 위해 연산자 오버로딩을 사용하였습니다.
```cpp
//Iterator.h
template<typename _Ty>
class Iterator final {
public:
	Node<_Ty>* operator*(void) const;
};
```
```cpp
//Iterator.cpp
template<typename _Ty>
Node<_Ty>* Iterator<_Ty>::operator*(void) const {
	return _cur;
}
```
\* 연산자 오버로딩을 사용하였습니다.<br>
이를 통해 접근시 반복자의 cur가 가리키는 node를 반환합니다.<br>
<br>
반복자가 node를 반환할 수 있게 되었습니다.<br>
마지막으로 반복자라는 이름에 걸맞게 노드를 반복 즉 순회할 수 있어야 합니다.<br>
이를 위해 또한번 연산자 오버로딩을 진행해 보겠습니다.
```cpp
//Iterator.h
template<typename _Ty>
class Iterator final {
public:
	void operator++(void);
	void operator--(void);
};
```
```cpp
//Iterator.cpp
template<typename _Ty>
void Iterator<_Ty>::operator++(void) {
 	_cur = _cur->_next;
}

template<typename _Ty>
void Iterator<_Ty>::operator--(void) {
	_cur = _cur->_prev;
}
```
증감 연산자를 오버로딩 하였습니다.<br>
cur의 next 또는 prev를 cur에 대입합니다.<br>
<br>
여기까지 반복자를 통해서 리스트의 노드를 탐색하는 기능을 완성하였습니다.

> ## 추가

자료 구조에서 가장 기본이 되는 삽입, 삭제, 탐색에 대해 구현하였습니다.
여기에서는 몇가지 추가적인 기능들에대해 필요성을 제기하고 구현해보겠습니다.
### 크기
배열은 자신이 가지고 있는 크기가 고정적이지만 리스트는 크기가 유동적으로 변합니다.
이에 리스트의 원소들을 탐색하지 않고 리스트의 사이즈만 알고 싶은 경우가 존재할 것입니다.

이러한 상황에서 매번 리스트를 돌며 갯수를 세는 것은 불필요한 자원 낭비일 것입니다.
따라서 size라는 변수를 추가하여 크기를 저장할 수 있습니다.
```cpp
//List.h
template<typename _Ty>
class List final {
private:
	size_t _size = 0;
};
```
구현은 너무 간단하기 때문에 정의부만 보고 넘어가겠습니다.
기존 push나 pop의 함수 끝에 증감 연산자를 사용해줍니다.
### 중간 삽입/삭제
리스트를 사용하다 보면 중간에 있는 값을 삭제하거나
혹은 중간에 값을 삽입하고 싶은 경우가 존재합니다.

이를 위해 중간 삽입/삭제 함수를 만들어 보겠습니다.

중간이라는 지점에 접근하기 위해서는 반복자를 사용해야 합니다.
허나 반복자 내에서 삭제가 이뤄지면 미리 만들어둔 size에 영향이 갑니다.
게다가 삽입과 삭제를 담당해야 하는 것은 리스트 클래스 입니다.

따라서 반복자를 매개변수로 받는 리스트 맴버함수를 제작해야합니다.
#### emplace
삽입 함수의 이름은 emplace입니다.
```cpp
//List.h
template<typename _Ty>
class List final {
public:
	void Emplace(Iterator<_Ty>& iter, const _Ty value);
};
```
```cpp
//List.cpp
template<typename _Ty>
void List<_Ty>::Emplace( Iterator<_Ty>& iter, const _Ty value) {
	Node<_Ty>* node = new Node<_Ty>{ value };

	Node<_Ty>* cur = (*iter);
	Node<_Ty>* next = cur->_next;

	cur->_next = node;
	node->_prev = cur;
	node->_next = next;
	next->_prev = node;

	++_size;
}
```
iter가 가리키는 cur노드와 그다음 next노드 사이에 새로운 node를 끼워넣습니다.
이를 수행하기 위해 각 연결고리를 수정해줘야 합니다.
위에 4줄되는 부분이 그에 해당하는데 순서에 맞춰 작성했습니다.
간단한 코드기 때문에 설명은 넘어가겠습니다.

### erase
삭제 함수의 이름은 erase입니다.
