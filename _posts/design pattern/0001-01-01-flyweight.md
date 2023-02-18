---
title: "경량(Flyweight)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

구조 디자인 패턴중 하나인 경량 패턴입니다.<br>
플라이급 패턴이라고도 불리며, 공유를 통해 많은 수의 객체들을 효과적으로 지원하는 패턴입니다.<br>
<br>
각 객체에 모든 데이터를 유지하는 대신 여러 객체들간의<br>
공통된 부분들을 공유하여 RAM에 더 많은 객체들을 포함할 수 있게 만드는 것입니다.<br>
> ## 필요성

게임을 구현하는데 있어 나무라는 객체를 많이 심기로 하였습니다.<br>
<br>
나무 객체에는 나무의 위치를 표현하기 위한 pos변수와<br>
나무의 모델을 표현하기 위한 model변수가 존재합니다.<br>
(모든 나무는 하나의 모델만을 사용합니다.)<br>
<br>
pos변수는 가벼운 데이터이지만,<br>
model변수는 몇천개의 폴리곤으로 구성돼있어 상당히 무겁습니다.<br>
<br>
이제 나무가 수백그루가 필요하다고 합니다.<br>
<br>
나무를 심고 실행해보니 상당히 무거워져 성능이 좋지못한 컴퓨터에서<br>
랙이 발생하거나 충돌이 나게 됩니다.<br>
(간단하게 구현하기 위해 pos와 model은 필요한 점의 개수를 가지는 정수형 변수로 작성하였습니다.)
```cpp
class Tree final {
private:
	int _pos = 1;
	int _model = 1000;
};

void main(void) {
	std::list<Tree> trees;

	for (int i = 0; i < 100; i++)
		trees.emplace_back(Tree{});
	//Tree객체가 100개 생성된다는 말은 model에 들어가는 점의 개수가 100000개 생성된다는 뜻입니다.
}
```
요즘 컴퓨터에서는 10만개의 점을 감당 못하지 않겠지만<br>
실제 모델링은 훨씬 더 많은 데이터를 가지고 있을 것입니다.<br>
또한 풀과 돌같은 객체들도 존재할 것이고 이는 수 천개가 필요할수도 있습니다.<br>
<br>
마지막으로 나무의 모델은 전부 같을텐데 데이터가 중복 생성되고 있습니다.
> ## 구현

이를 해결하기 위해서 경량 패턴을 사용해보겠습니다.<br>
경량패턴에서는 각 객체들이 가지는 상태를 두가지로 분리합니다.
- 본질적/고유 상태(intrinsic)
- 부가적/외부 상태(extrinsic)

본질적 상태는 각 객체들이 처한 상황에 관계 없이 본질적인 상태를 유지해야하는 정보를 말합니다.<br>
이러한 정보는 공유될 수 있습니다. 코드에서는 model이 본질적 상태에 해당합니다.<br>
<br>
부가적 상태는 객체가 사용될 상황에 따라 달라질수 있고 그 상황에 종속적인 정보를 말합니다.<br>
이러한 정보는 공유될 수 없습니다. 코드에서는 pos가 부가적 상태에 해당합니다.<br>
<br>
경량 패턴은 본질적 상태에 해당하는 데이터를 공유하여 메모리 사용량을 감소시킵니다.
```cpp
class Model final {
private:
	int _model = 1000;
};

class Tree final {
public:
	Tree(Model* model) {
		_model = model;
	}
private:
	int _pos = 1;
	Model* _model = nullptr;
};

void main(void) {
	Model* model = new Model;
	std::list<Tree> trees;

	for (int i = 0; i < 100; i++)
		trees.emplace_back(Tree{ model }); //이제 모든 나무는 하나의 모델 객체를 공유합니다.
	//따라서 생성되는 model의 점의 개수는 1000개 입니다.
	delete model;
}
```
이제 필요한 model의 점의 개수는 1000개면 충분해지기 때문에 pos의 데이터를 저장할<br>
공간만 있다면 객체를 생성할 수 있습니다.

