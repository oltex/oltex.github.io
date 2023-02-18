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
<br>
pos변수는 가벼운 데이터이지만,<br>
model변수는 몇천개의 폴리곤으로 구성돼있어 상당히 무겁습니다.<br>
<br>
이제 나무가 수백그루가 필요하다고 합니다.<br>
<br>
나무를 심고 실행해보니 상당히 무거워져 성능이 좋지못한 컴퓨터에서<br>
랙이 발생하거나 충돌이 나게 됩니다.<br>
<br>
(간단하게 구현하기 위해 pos와 model은 필요한 점의 개수를 가지는 정수형 변수로 작성하였습니다.<br>
그 만큼 많은 변수가 있다고 생각해주시면 되겠습니다.<br>
점의 구조체는 다이렉트x에서 사용하는 XMFLOAT3(float x , y, z)를 사용했습니다.<br>
int pos = 1 -> XMFLOAT3가 1개 있다 -> float 변수가 3개 있다.<br>
int model = 1000 -> XMFLOAT3가 1000개 있다 -> float 변수가 3000개 있다.)
```cpp
class Tree final {
public:
	void Render(void) {
	}
private:
	int pos = 1;
	long long model = 1000;
};

void main(void) {
	std::list<Tree> trees;

	for (int i = 0; i < 100; i++)
		trees.emplace_back(Tree{});
	//Tree객체가 100개 생성된다는 말은 model에 들어가는 float의 개수가 1000 * 3 * 100 개 생성된다는 뜻입니다.
}
```


> ## 구현

