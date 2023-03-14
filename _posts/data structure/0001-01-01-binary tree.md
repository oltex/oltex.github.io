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
n개의 자식을 가질 수 있는 트리 구조에서,<br>
LCRS(left-child, right-sibling)방식을 통해 모든 트리를 이진 트리 형태로 재구성할 수 있습니다.<br>
때문에 특별한 이유가 없는 이상 트리는 보통 이진 트리로 구현됩니다.
> ## 종류

이진 트리에는 이진 트리의 특정 상태에 따라 불리는 이름들이 존재합니다.
- 진 이진 트리(full binary tree):<br>
모든 트리의 자식은 0개나 2개인 트리를 의미합니다.
- 포화 이진 트리(perfect binary tree):<br>
모든 리프 노드의 높이가 같고 리프 노드가 아닌 노드는 모두 2개의 자식을 갖는 트리를 의미합니다.<br>
트리의 높이가 n일 때 가장 많이 존재할 수 있는 노드의 수인 2^n - 1개를 모두 채운 이진 트리를 의미합니다.
모든 포화 이진 트리는 정 이진 트리입니다.
- 완전 이진 트리(complete binary tree):<br>
모든 리프 노드의 깊이가 최대 1 차이가 나고, 모든 노드의 오른쪽 자식이 있으면 왼쪽 자식이 있는 이진 트리를 의미합니다.<br>
다시 말해 노드가 아래서 위로, 왼쪽에서 오른쪽으로 하나씩 빠짐없이 채워나간 형태를 의미합니다.<br>
포화 이진 트리는 완전 이진 트리의 부분집합입니다.

> ## 구조

이진트리의 구조는 다음과 같습니다.
- Node: 이진 트리를 구성하는 데이터들의 집합체입니다.
각 노드는 left/right를 구분하는 노드 참조자가 존재합니다.
- Tree: 트리를

> ## 구현

이진트리를 구현해 보겠습니다.
이진 트리는 재귀적인 특성을 지니고 있기 때문에 일부 연산이 재귀 호출의 형태를 띕니다.

### 노드
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

### 메인
```cpp
void main(void) {
	Node<int>* node = new Node<int>{ 10 };
	Node<int>* left_node = new Node<int>{ 20 };
	Node<int>* right_node = new Node<int>{ 30 };

	node->_left = left_node;
	node->_right = right_node;

	std::cout << node->_data << std::endl;
	std::cout << node->_left->_data << std::endl;
	std::cout << node->_right->_data << std::endl;

	delete left_node;
	delete right_node;
	delete node;
}
```
