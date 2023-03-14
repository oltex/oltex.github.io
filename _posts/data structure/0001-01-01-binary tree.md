---
title: "이진 트리(Binary Tree)"
categories:
  - data structure
tags:
  - tag
---
> ## 정의

이진 트리란 다음과 같은 조건을 만족하는 트리를 의미합니다.
- 루트 노드를 중심으로 두개의 서브 트리로 나눠집니다.
- 나눠진 두 서브 트리도 모두 이진 트리여야 합니다.
- 노드가 위치할 수 있는 공간에 노드가 존재하지 않는다면 공집합 노드가 존재하는 것으로 간주합니다.

정리하자면,<br>
이진 트리는 자식 노드의 개수를 최대 2개로 제한하는 트리 라고 볼 수 있습니다.<br>
<br>
이진 트리는 트리의 가장 간단한 형태이며<br>
두 자식 노드를 왼쪽/오른족 자식으로 구분짓습니다.<br>
<br>
이진 트리를 사용하는 이유는<br>
n개의 자식을 가질 수 있는 트리 구조에서, LCRS(left-child, right-sibling)방식을 통해<br>
모든 트리를 이진 트리 형태로 재구성할 수 있습니다.<br>
때문에 특별한 이유가 없는 이상 트리는 보통 이진 트리로 구현됩니다.
> ## 구조

이진트리의 구조는 다음과 같습니다.
- Node: 이진 트리를 구성하는 데이터들의 집합체입니다.<br>
각 노드는 left/right를 구분하는 노드 참조자가 존재합니다.
- Tree: 이진 트리를 구성하는 클래스 입니다.<br>
트리의 목적에 따라 노드를 정렬하고 탐색하는 기능이 존재할 수 있습니다.<br>
트리에는 루트 노드를 참조할 root 포인터가 존재합니다.

트리는 연결 리스트와 다르게 자료의 저장 방식에 초점을 두지않고,<br>
자료의 표현에 초점을 두기 때문에<br>
아무런 목적이 없는 트리의 구성에는 별도의 함수를 필요로 하지 않습니다.<br>
<br>
단, 모든 트리에는 순회/탐색과 삭제 기능은 존재해야 합니다.
> ## 순회

이진 트리의 구현에 앞서, 순회에 대해 알아보겠습니다.<br>
이진 트리는 노드를 순회할 수 있는 방법을 제공해야 합니다.<br>
이진 트리를 대상으로 하는 대표적인 순회 방법은 다음과 같습니다.
- 전위 순회(preorder traversal)
- 중위 순회(inorder traversal)
- 후위 순회(postorder traversal)

이들의 명칭은 각각 root노드를 언제 탐색 하느냐에 따라 붙여졌습니다.<br>

다음과 같은 트리가 있다고 가정해 보겠습니다.<br>
![alt](/assets/images/data-structure/0001-01-01-binary-tree/1.png)<br>
전위 순회는 루트 노드를 가장 먼저 방문하는 순회 방법입니다.<br>
A, B, C 순으로 순회를 진행합니다.<br>
<br>
중위 순회는 루트 노드를 중간에 방문하는 순회 방법입니다.<br>
B, A, C 순으로 순회를 진행합니다.<br>
<br>
후위 순회는 루트 노드를 마지막으로 방문하는 순회입니다.<br>
B, C, A 순으로 순회를 진행합니다.<br>
<br>
각 순회 방식에 해당하는 자식 노드 B, C는 서브 트리일 수 있습니다.<br>
그러면 각 서브 트리에게도 앞서 말한 순회 방식을 적용시킵니다.<br>
따라서, 순회 방식은 재귀적으로 만들어집니다.
> ## 구현

이진 트리를 구현해 보겠습니다.<br>
이진 트리는 재귀적인 특성을 지니고 있기 때문에 일부 연산이 재귀 호출의 형태를 띕니다.<br>
<br>
이진 트리를 노드 구조체와 트리 클래스 + 반복자 패턴 그리고 메인 함수로<br>
구성된 코드로 작성해보도록 하겠습니다.
### 노드 구조체
```cpp
template<typename _Ty>
struct Node final {
	explicit Node(const _Ty& data) :
		_data(data) {
	}
	_Ty _data;
	Node* _left = nullptr;
	Node* _right = nullptr;
};
```
노드 구조체입니다.<br>
노드에는 저장할 자료를 담을 data변수와<br>
자신의 자식 노드를 참조할 left, right 포인터가 존재합니다.
### 트리 클래스
```cpp
template<typename _Ty>
class Tree final {
public:
	enum class Traversal : unsigned char {
		PREORDER, INORDER, POSTORDER
	};
	using Iterator = Iterator<_Ty>;
public:
	Tree(Node<_Ty>* root);
	~Tree(void);
public:
	Iterator* Begin(const Traversal& traversal);
protected:
	void Delete(const Node<_Ty>* node);
private:
	Node<_Ty>* _root = nullptr;
};
```
```cpp
template<typename _Ty>
Tree<_Ty>::Tree(Node<_Ty>* root) :
	_root(root) {
}

template<typename _Ty>
Tree<_Ty>::~Tree(void) {
	Delete(_root);
}

template<typename _Ty>
Iterator<_Ty>* Tree<_Ty>::Begin(const Traversal& traversal) {
	switch (traversal) {
	case Traversal::PREORDER:
		return new Preorder<_Ty>{ _root };
	case Traversal::INORDER:
		return new Inorder<_Ty>{ _root };
	case Traversal::POSTORDER:
		return new Postorder<_Ty>{ _root };
	}
	return nullptr;
}

template<typename _Ty>
void Tree<_Ty>::Delete(const Node<_Ty>* node) {
	if (nullptr == node)
		return;
	Delete(node->_left);
	Delete(node->_right);
	delete node;
}
```
트리 클래스 입니다.<br>
트리에는 루트 노드를 저장할 root 포인터가 존재합니다.<br>
<br>
트리의 순회를 위해 반복자(iterator) 패턴을 사용하였습니다.<br>
begin에서 enum Traversal의 PREORDER, INORDER, POSTORDER 값에 따라<br>
iterator반환으로 Preorder, Inorder, Postorder 중 하나의 구체 클래스를 갖게 됩니다.<br>
<br>
트리의 소멸자에 사용되는 delete함수가 존재합니다.<br>
재귀 함수를 사용하여 후위 순회하면서 삭제하는 함수입니다.
### 반복자 클래스
```cpp
template<typename _Ty>
class Iterator abstract {
public:
	Iterator(Node<_Ty>* node) :
		_node(node) {
	}
public:
	void Print(void);
protected:
	virtual void Print(const Node<_Ty>* node) = 0;
protected:
	Node<_Ty>* _node;
};

template<typename _Ty>
class Preorder final : public Iterator<_Ty> {
public:
	Preorder(Node<_Ty>* node) :
		Iterator<_Ty>(node) {
	}
protected:
	virtual void Print(const Node<_Ty>* node) override;
};

template<typename _Ty>
class Inorder final : public Iterator<_Ty> {
public:
	Inorder(Node<_Ty>* node) :
		Iterator<_Ty>(node) {
	}
protected:
	virtual void Print(const Node<_Ty>* node) override;
};

template<typename _Ty>
class Postorder final : public Iterator<_Ty> {
public:
	Postorder(Node<_Ty>* node) :
		Iterator<_Ty>(node) {
	}
protected:
	virtual void Print(const Node<_Ty>* node) override;
};
```
```cpp
template<typename _Ty>
void Iterator<_Ty>::Print(void) {
	Print(_node);
}

template<typename _Ty>
void Preorder<_Ty>::Print(const Node<_Ty>* node) {
	if (nullptr == node)
		return;
	std::cout << node->_data << std::endl;
	Print(node->_left);
	Print(node->_right);
}

template<typename _Ty>
void Inorder<_Ty>::Print(const Node<_Ty>* node) {
	if (nullptr == node)
		return;
	Print(node->_left);
	std::cout << node->_data << std::endl;
	Print(node->_right);
}

template<typename _Ty>
void Postorder<_Ty>::Print(const Node<_Ty>* node) {
	if (nullptr == node)
		return;
	Print(node->_left);
	Print(node->_right);
	std::cout << node->_data << std::endl;
}
```
트리의 순회를 구현한 반복자 클래스 입니다.<br>
<br>
반복자 클래스는 총 3가지로 전위 중위 후위 순회를 구현하였습니다.<br>
반복자 클래스의 Print함수를 실행하면 재귀 함수가 실행되며<br>
내부의 노드를 순회하며 출력합니다.
### 메인 함수
```cpp
void main(void) {
	Node<int>* node = new Node<int>{ 10 };
	Node<int>* left_node = new Node<int>{ 20 };
	Node<int>* right_node = new Node<int>{ 30 };

	node->_left = left_node;
	node->_right = right_node;

	Tree<int> tree{ Tree<int>::TRAVERSAL::PREORDER, node }; //PREORDER, INORDER, POSTORDER 중 하나가 들어갈 수 있습니다.
	tree.Print();
}
```
메인 함수입니다.<br>
<br>
노드를 node, left_node, right_node 총 3개 구현하였습니다.<br>
이들에게 각각 10, 20, 30의 데이터를 담고<br>
node의 자식 노드로 left_node와 right_node를 연결하였습니다.<br>
<br>
이후 이를 사용하여 트리 클래스를 만듭니다.<br>
트리 클래스는 root노드 포인터를 받게됩니다.<br>
트리 클래스의 노드들을 순회하기 위해 반복자 패턴을 사용했습니다.<br>
<br>
노드의 삽입이 연결 리스트와 다르게 트리에서 이루어지지 않는 이유는<br>
트리의 목적이 존재하지 않기 때문에 어떤 방식으로 삽입할지<br>
트리가 결정할 수 없기 때문입니다.<br>
트리의 목적이 결정되면 트리가 제어할 수 있게 됩니다.
> ## 종류

이진 트리에는 이진 트리의 특정 상태에 따라 불리는 이름들이 존재합니다.
- 진 이진 트리(full binary tree):<br>
모든 트리의 자식은 0개나 2개인 트리를 의미합니다.
- 포화 이진 트리(perfect binary tree):<br>
모든 리프 노드의 높이가 같고 리프 노드가 아닌 노드는 모두 2개의 자식을 갖는 트리를 의미합니다.<br>
트리의 높이가 n일 때 가장 많이 존재할 수 있는 노드의 수인 2^n - 1개를 모두 채운 이진 트리를 의미합니다.<br>
모든 포화 이진 트리는 정 이진 트리입니다.
- 완전 이진 트리(complete binary tree):<br>
모든 리프 노드의 깊이가 최대 1 차이가 나고, 모든 노드의 오른쪽 자식이 있으면 왼쪽 자식이 있는 이진 트리를 의미합니다.<br>
다시 말해 노드가 아래서 위로, 왼쪽에서 오른쪽으로 하나씩 빠짐없이 채워나간 형태를 의미합니다.<br>
포화 이진 트리는 완전 이진 트리의 부분집합입니다.
