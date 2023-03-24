---
title: "이진 탐색 트리(Binary Search Tree)"
categories:
  - data structure
tags:
  - tag
---
> ## 개요

이진 트리 자료구조 중 하나로<br>
효율적인 탐색을 위해 고안된 자료 구조입니다.<br>
<br>
노드의 왼쪽 서브 트리는 노드의 값보다 작은 값들을 지닌 노드로 이루어져 있습니다.<br>
노드의 오른쪽 서브 트리는 노드의 값보다 큰 값들을 지닌 노드로 이루어져 있습니다.
> ## 구조

### 노드
이진 탐색 트리의 노드는 2가지 데이터를 가지고 있습니다.
- key: 노드의 탐색 및 식별에 사용될 고유 번호를 의미합니다.
- value: 노드의 데이터를 저장한 값을 의미합니다.

노드의 데이터중 키는 다른 노드와 중복될 수 없습니다.<br>
이진 탐색 트리의 노드에 존재하는 키를 사용하여 탐색을 진행해 나갑니다.
### 트리
이진 탐색 트리는 효율적인 탐색을 위한 노드의 저장 규칙이 존재합니다.<br>
<br>
모든 노드에 대하여,<br>
노드의 왼쪽 서브 트리는 노드보다 작은 키값을 가집니다.<br>
노드의 오른쪽 서브 트리는 노드보다 큰 키값을 가집니다.<br>
<br>
이렇게 구성해 둔다면 어떤 키 n을 찾을 때,<br>
루트 노드의 키와 비교하여 n이 더 작다면 루트 노드의<br>
오른쪽 서브 트리에 대해서는 탐색할 필요가 없어집니다.<br>
<br>
이진 탐색 트리에 사용되는 함수는 4가지 입니다.
- insert: 노드를 삽입합니다.
- Find: 특정한 노드를 탐색하는 함수입니다.
- Traverse: 모든 노드를 순회하며 출력합니다.
- Delete: 노드를 삭제합니다.

### 삽입
이진 탐색 트리의 데이터 삽입은 앞서 얘기한 규칙에 근거하여<br>
삽입될 노드가 루트 노드 부터 시작하여 자신의 자리를 찾아가는 과정을 거칩니다.<br>
<br>
먼저 루트 노드와의 비교를 통해 삽입될 노드의 키가 작은지 큰지 판단합니다.<br>
키가 작다면 왼쪽, 키가 크다면 오른쪽 자식 노드로 이동합니다.<br>
이 과정을 반복하여 비어 있는 자리가 오면 삽입합니다.
### 탐색
이진 탐색 트리의 탐색 과정은 삽입 과정과 똑같습니다.<br>
탐색할 키값을 가지고 루트 노드부터 시작하여 노드를 찾아가는 과정을 거칩니다.<br>
<br>
먼저 루트 노드와의 비교를 통해 탐색할 키가 작은지 큰지 판단합니다.<br>
키가 작다면 왼쪽, 키가 크다면 오른쪽 자식 노드로 이동합니다.<br>
이 과정을 반복하여 키가 같은 노드를 찾으면 중단합니다.
### 순회
이진 탐색 트리의 순회는 중위 순회를 사용하여 구현합니다.<br>
중위 순회를 사용하면 트리 내의 모든 노드들이 정렬되어 나오게 됩니다.
### 삭제
이진 탐색 트리의 삭제 과정은 특정 노드를 삭제한 후<br>
이진 탐색 트리의 규칙에 맞게 다시 정렬해야 되는 과정을 거처야 합니다.<br>
<br>
하지만 모든 상황에서 같은 정렬 방식을 사용하지 않습니다.<br>
삭제할 노드의 상태에따라 정렬하는 방식이 달라지게 됩니다.<br>
이는 다음과 같이 두 가지로 나뉠 수 있습니다.
1. 삭제할 노드의 자식 노드가 0~1개일 때

단말 노드또는 자식이 한개인 노드라면 정렬에 큰 신경을 쓸 필요가 없습니다.<br>
남은 자식의 노드를 삭제할 노드의 위치에 넣는다면 모든 정렬이 완료되게 됩니다.

2. 삭제할 노드의 자식 노드가 2개일 때

노드의 자식이 2개일 경우 왼쪽 또는 오른쪽 서브 트리 내에서<br>
가장 크거나 작은 값을 찾아서 대체할 노드로 잡고, 삭제할 노드의 위치에 넣어줘야 합니다.<br>
(왼쪽 서브 트리를 선택했다면 가장 큰 값을,<br>
오른쪽 서브 트리를 선택했다면 가장 작은 값을 넣어줍니다.)

> ## 구현

이진 탐색 트리를 구현해 보겠습니다.
### 노드
```cpp
template<typename _Kty, typename _Ty>
struct Node final {
	explicit Node(const _Kty& key, const _Ty& value) :
		_key(key), _value(value) {
	}
	_Kty _key;
	_Ty _value;
	Node* _left = nullptr;
	Node* _right = nullptr;
};
```
이진 탐색 트리에 사용될 노드 구조체 입니다.<br>
<br>
노드 구조체에는 각 노드의 키와 벨류 값이 존재합니다.<br>
그리고 노드의 왼쪽과 오른쪽 자식 노드를 참조할 포인터가 존재합니다.
### 트리
```cpp
template<typename _Kty, typename _Ty>
class Tree final {
public:
	~Tree(void);
public:
	void Insert(const _Kty& key, const _Ty& data);
	void Erase(const _Kty& key);
	const _Ty Find(const _Kty& key);
	void Traverse(void);
protected:
	void Traverse(Node<_Kty, _Ty>* node);
	void Delete(Node<_Kty, _Ty>* node);
private:
	Node<_Kty, _Ty>* _root = nullptr;
};
```
이진 탐색 트리 클래스 입니다.<br>
루트 노드를 저장하기 위한 root 포인터 변수가 존재합니다.<br>
<br>
구조 절에서 설명된 함수들은 public에<br>
Insert, Erase, Find, Traverse란 이름으로 존재합니다.<br>
<br>
추가로 protected로 존재하는 함수들은 다음과 같습니다.
- Traverse: 중위 순회 재귀 함수입니다. 노드를 출력하기 위해 존재합니다. public Traverse에서 호출됩니다.
- Delete: 후위 순회 재귀 함수입니다. 노드를 삭제하기 위해 존재합니다. 소멸자에서 호출됩니다.

(소멸자와 순회는 따로 작성하지 않았습니다.)
#### 삽입
```cpp
template<typename _Kty, typename _Ty>
void Tree<_Kty, _Ty>::Insert(const _Kty& key, const _Ty& data) {
	Node<_Kty, _Ty>* parent = nullptr;
	Node<_Kty, _Ty>* child = _root;
	while (nullptr != child) {
		parent = child;
		if (key == child->_key)
			return;
		else if (key < child->_key)
			child = child->_left;
		else
			child = child->_right;
	}

	Node<_Kty, _Ty>* node = new Node<_Kty, _Ty>{ key, data };
	if (nullptr == _root)
		_root = node;
	else
		if (node->_key < parent->_key)
			parent->_left = node;
		else
			parent->_right = node;
}
```
노드의 삽입을 위한 Insert 함수입니다.<br>
child를 root로 설정하고 시작합니다.<br>
<br>
첫번째 while문을 통해 계속해서 child를 찾아 내려갑니다.<br>
만약 같은 key값을 가진 노드가 나온다면 삽입하지 않고 리턴합니다.<br>
<br>
만약 child가 nullptr이 된다면 넣을 자리를 찾았다는 얘기가 됩니다.<br>
따라서 노드 node를 생성하고 parent의 left나 right에 이를 삽입합니다.
#### 탐색
```cpp
template<typename _Kty, typename _Ty>
const _Ty Tree<_Kty, _Ty>::Find(const _Kty& key) {
	Node<_Kty, _Ty>* find = _root;

	while (nullptr != find) {
		if (key == find->_key)
			return find->_value;
		else if (key < find->_key)
			find = find->_left;
		else
			find = find->_right;
	}
	return _Ty{};
}
```
노드의 탐색을 위한 Find 함수입니다.<br>
find를 root로 설정하고 시작합니다.<br>
<br>
첫번째 while문을 통해 계속해서 find를 찾아 내려갑니다.<br>
만약 같은 key값을 가진 노드가 나온다면 그 노드의 value를 리턴합니다.<br>
나오지 않는다면 빈 노드를 리턴합니다.
#### 삭제
```cpp
template<typename _Kty, typename _Ty>
void Tree<_Kty, _Ty>::Erase(const _Kty& key) {
	Node<_Kty, _Ty>* parent = nullptr;
	Node<_Kty, _Ty>* erase = _root;

	while (nullptr != erase && key != erase->_key) {
		parent = erase;
		if (erase->_key > key)
			erase = erase->_left;
		else
			erase = erase->_right;
	}
	if (nullptr == erase)
		return;

	Node<_Kty, _Ty>* child = nullptr;
	if (nullptr != erase->_left && nullptr != erase->_right) {
		child = erase->_right;
		if (nullptr != child->_left) {
			Node<_Kty, _Ty>* parent = nullptr;
			while (nullptr != child->_left) {
				parent = child;
				child = child->_left;
			}
			parent->_left = child->_right;
			child->_right = erase->_right;
		}
		child->_left = erase->_left;
	}
	else {
		if (nullptr != erase->_left)
			child = erase->_left;
		else
			child = erase->_right;
	}

	if (nullptr == parent)
		_root = child;
	else
		if (parent->_left == erase)
			parent->_left = child;
		else
			parent->_right = child;

	delete erase;
}
```
노드의 삭제를 위한 erase 함수입니다.<br>
erase를 root로 설정하고 시작합니다.<br>
<br>
세가지 과정으로 나눠서 설명하겠습니다.
##### 탐색
```cpp
	while (nullptr != erase && key != erase->_key) {
		parent = erase;
		if (erase->_key > key)
			erase = erase->_left;
		else
			erase = erase->_right;
	}
	if (nullptr == erase)
		return;
```
while문을 통해 erase가 nullptr이 되거나,<br>
erase의 key가 삭제할 노드의 key와 맞을 때까지 반복합니다.<br>
<br>
만약 erase가 nullptr이 된다면<br>
삭제할 노드의 key를 찾지 못했다는 뜻이기 때문에 리턴합니다.
##### 대체
```cpp
	Node<_Kty, _Ty>* child = nullptr;
	if (nullptr != erase->_left && nullptr != erase->_right) {
		child = erase->_right;
		if (nullptr != child->_left) {
			Node<_Kty, _Ty>* parent = nullptr;
			while (nullptr != child->_left) {
				parent = child;
				child = child->_left;
			}
			parent->_left = child->_right;
			child->_right = erase->_right;
		}
		child->_left = erase->_left;
	}
	else {
		if (nullptr != erase->_left)
			child = erase->_left;
		else
			child = erase->_right;
	}
```
삭제할 노드 erase를 찾았다면 삭제를 잠시 미뤄두고<br>
대체할 노드 child를 찾아야 합니다.<br>
```cpp
	else {
		if (nullptr != erase->_left)
			child = erase->_left;
		else
			child = erase->_right;
	}
```
else 문 부터 설명하자면<br>
erase의 자식 노드가 0~1개인 경우입니다.<br>
이 때 erase의 left가 nullptr이라면 right를 child로 선택합니다.<br>
(만약 자식이 0개라도 right는 nullptr이기 때문에 nullptr로 선택한 것과 다름이 없습니다.)<br>
```cpp
	if (nullptr != erase->_left && nullptr != erase->_right) {
		child = erase->_right;
		if (nullptr != child->_left) {
			Node<_Kty, _Ty>* parent = nullptr;
			while (nullptr != child->_left) {
				parent = child;
				child = child->_left;
			}
			parent->_left = child->_right;
			child->_right = erase->_right;
		}
		child->_left = erase->_left;
	}
```
다음은 if문 입니다.<br>
erase의 자식 노드가 2개인 경우입니다.<br>
이 때는 child를 찾기 위해 왼쪽/오른족 중 한방향을 선택해야 하는데 무조껀 right를 선택해 주었습니다.<br>
따라서 child를 erase->\_right로 설정하고 시작합니다.<br>
<br>
1.<br>
erase의 자리에 child가 들어가야 하기 때문에<br>
child의 left를 erase의 left로 채워줍니다.(마지막 문단)<br>
<br>
2.<br>
child의 right또한 erase의 right로 채워줘야합니다.<br>
만약 child가 erase의 오른쪽 자식 노드라면 이 작업이 이뤄지지 않습니다.<br>
<br>
만약 child가 erase의 오른쪽 자식 노드가 아니라면 (즉, child의 왼쪽 자식 노드가 있다면)<br>
while문을 통해 제일 마지막 왼쪽 자식 노드를 찾아 child로 설정해줍니다.<br>
이후 child의 right를 erase의 right로 설정해 줍니다.<br>
<br>
이 상황에서 찾은 child를 erase자리로 올리려면 parent의 left가 망가지게 되므로<br>
이를 child의 right로 설정해 줘야 합니다.
##### 연결
```cpp
	if (nullptr == parent)
		_root = child;
	else
		if (parent->_left == erase)
			parent->_left = child;
		else
			parent->_right = child;

	delete erase;
```
찾은 child를 erase의 parent와 연결하고<br>
erase를 삭제하는 작업입니다.<br>
<br>
만약 parnet가 nullptr이라면 삭제할 erase가 root라는 뜻이 되므로<br>
child를 루트로 저장합니다.
> ## 사용

구현한 이진 탐색 트리를 사용해보겠습니다.
```cpp
void main(void) {
	Tree<int, int> tree;

	tree.Insert(12, 120);
	tree.Insert(17, 170);
	tree.Insert(21, 210);
	tree.Insert(13, 130);
	tree.Insert(7, 70);
	tree.Insert(4, 40);
	tree.Insert(2, 20);
	tree.Insert(9, 90);
	tree.Insert(10, 100);
	tree.Insert(8, 80);

	tree.Erase(7);
	std::cout << tree.Find(13) << std::endl;
}
```
이진 탐색 트리를 사용한 메인 함수입니다.<br>
노드 삽입, 삭제 탐색이 이루어졌습니다.
> ## 복잡도
이진 탐색 트리의 시간 복잡도에 대해 얘기해 보겠습니다.<br>
<br>
이진 탐색 트리는 탐색은<br>
탐색이 한번 이루어질 때마다 노드의 갯수가 절반으로 줄어드는<br>
특징을 가지고 있으므로 탐색의 시간 복잡도는 O(log2n)입니다.<br>
<br>
이진 탐색 트리의 삽입은 탐색이 동반됩니다.<br>
마찬가지로 탐색할 노드가 절반씩 줄어드는 특징을 가지고 있으므로<br>
삽입의 시간 복잡도 또한 O(log2n)입니다.
### 단점
이진 탐색 트리는 한가지 단점을 지니고 있습니다.<br>
예를 들어 다음과 같이 노드를 집어넣은 트리의 경우입니다.
```cpp
	tree.Insert(1, 10);
	tree.Insert(2, 20);
	tree.Insert(3, 30);
	tree.Insert(4, 40);
	tree.Insert(5, 50);
  ...
```
이러한 경우 노드가 한쪽으로 치우쳐져 있게되고 이른 불균형하다고 표현합니다.<br>
트리의 노드구조가 불균형 할 수록 탐색 시간이 길어져 최악의 경우 O(n)의 시간 복잡도를 가지게 됩니다.
