---
title: "컴포넌트(Component)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

컴포넌트 디자인 패턴입니다.<br>
객체가 여러 분야의 객체를 커플링 없이 다룰 수 있게 합니다.
> ## 필요성

컴포넌트 패턴은 다음과 같은 상황에서 사용하면 좋습니다.
- 한 클래스에서 여러 분야를 건드리고 있어서, 이들을 디커플링하고 싶다.
- 클래스가 거대해져서 작업하기 힘들다.
- 여러 기능을 공유하는 객체를 정의하고 싶다.

게임을 만들다 보면 한 객체가 여러 분야를 다뤄야하는 경우가 존재합니다.<br>
예를 들어 Player클래스를 생성했다고 가정해보겠습니다.<br>
<br>
플레이어는 Update함수를 매프레임 호출하며<br>
자신의 로직을 처리합니다.<br>
<br>
플레이어는 다음과 같은 기능을 가지고 있습니다.
- 위치를 저장하고 이를 이동할 수 있습니다.
- 물리를 적용 받으며 충돌 판정을 처리합니다.
- 모델을 관리/동작시킵니다.

이를 코드로 구현하면 다음과 같습니다.
```cpp
class Player final {
public:
	void Update(void) {
		//위치를 처리합니다.
		//물리를 처리합니다.
		//모델을 처리합니다.
	}
private:
	//위치에 관한 변수
	//물리에 관한 변수
	//모델에 관한 변수
};
```
플레이어 클래스 내에 모든 코드를 집어넣다 보니<br>
코드가 길어지는 문제가 발생했습니다.<br>
<br>
길어지는 문제보다 더 심각한 문제는 서로의 코드들이 서로<br>
커플링 되어 있다는 것입니다.
> ## 구현

이 상태로는 코드를 제대로 수정/제작 하기 어렵습니다.<br>
따라서 서로의 코드들을 디커플링 시키는 방법을 찾아야 합니다.<br>
<br>
컴포넌트 패턴은 이러한 행동들을 다른 클래스(컴포넌트 클래스)로 추출합니다.<br>
컴포넌트들은 서로 디커플링 되어있습니다.<br>
<br>
먼저 컴포넌트 클래스를 제작해 보겠습니다.
```cpp
class Transform final {
public:
	void Update(void) {
		//위치를 처리합니다.
	}
private:
	//위치에 관한 변수
};
```
```cpp
class Collider final {
public:
	void Update(void) {
		//물리를 처리합니다.
	}
private:
	//물리에 관한 변수
};
```
```cpp
class Model final {
public:
	void Update(void) {
		//모델을 처리합니다.
	}
private:
	//모델에 관한 변수
};
```
이제 각 컴포넌트들은 자신이 맏은 동작만 처리하게 됩니다.<br>
<br>
이제 이들을 플레이어 클래스와 결합시켜야 합니다.<br>
(간단하게 하기위해 컴포넌트 내부, 소멸자, delete를 제외하였습니다.)
```cpp
class Player final {
public:
	Player(Transform* transform, Collider* collider, Model* model) :
		_transform(transform), _collider(collider), _model(model) {
	}
	void Update(void) {
		_transform->Update();
		_collider->Update();
		_model->Update();
	}
private:
	Transform* _transform = nullptr;
	Collider* _collider = nullptr;
	Model* _model = nullptr;
};
```
이제 플레이어는 단순히 컴포넌트들의 컨테이너 역할만을 담당합니다.
> ## 구현
