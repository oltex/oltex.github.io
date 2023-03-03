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
> ## 추가

아직 해결되지 않은 문제가 남아있습니다.<br>
바로 각 컴포넌트들 끼리는 상호작용을 하는 경우가 있다는 것입니다.<br>
<br>
위의 예를 들어<br>
물리 컴포넌트는 플레이어가 물체와 부딛혔을때 위치를 밀어내야 할 것입니다.<br>
이를 위해서 Transform과 Collider 컴포넌트는 서로 상호작용이 필요합니다.<br>
<br>
해결 방안은 3가지가 존재합니다.
### 컨테이너 이용
컨테이너(Player)를 이용하는 방법입니다.<br>
각 컴포넌트는 기존 방식대로 행동하고 이에 대한 결과를 Player가 처리하는 방법입니다.
```cpp
class Player final {
public:
	void Update(void) {
		if(_collider->IsCollision()) //1. 만약 충돌 했다면,
			_transform->SetPos(); //2. 위치를 바꿉니다.
	}
};
```
이렇게 하면 컴포넌트들 끼리는 디커플링 상태를 유지할 수 있습니다.<br>
허나 컴포넌트들이 공유하는 정보를 컨테이너에 넣어야하고<br>
컴포넌트들의 실행 순서에 영향을 받게됩니다.
### 컴포넌트 참조
각 컴포넌트들이 객체를 통하지 않고 참조하는 방식입니다.<br>
Collider의 IsCollision함수에 매개변수로 Transform을 넘기는 방식입니다.
```cpp
class Collider final {
public:
	void IsCollision(Transform* transform) { //위치를 매개변수로 받습니다.
		if (true) //1. 만약 충돌 했다면,
			transform->SetPos(); //2. 위치를 바꿉니다. 
	}
};
```
컴포넌트들 끼리 상호작용 하기 때문에 상호 작용이 간단하고 빠릅니다.(제약이 없습니다.)<br>
허나 컴포넌트들끼리 강한 커플링이 생기게 됩니다.
### 중재자
중재자 패턴을 사용하는 방법도 존재합니다.<br>
<br>
중재자 패턴의 정의를 한번 살펴보겠습니다.<br>
객체들간의 직접적인 통신을 제한하고 중재자를 통해 통신함으로써 의존성을 줄입니다.<br>
<br>
여기서 객체들은 컴포넌트들이 될 것이고<br>
중재자는 이미 존재하는 플레이어 객체를 사용하면 될 것입니다.<br>
<br>
이를 위해 Player의 상위 클래스로 Object를<br>
컴포넌트들의 상위 클래스로 Component를 만들어보겠습니다.<br>
(맴버 변수들은 어떤 방식으로든 제대로 채워졌다고 가정하겠습니다.)
```cpp
class Object abstract {
public:
	void notify(std::string message) { //메세지를 받는 알림 함수 입니다.
		for (auto& iter : _components) //알림이 들어오면
			iter->Notify(message); //모든 컴포넌트에 알림을 보냅니다.
	}
protected:
	std::vector<Component*> _components; //자신의 모든 컴포넌트
};
```
오브젝트 클래스에 알림 함수를 정의합니다.<br>
알림이 들어오면 모든 컴포넌트에 같은 알림을 보냅니다.
```cpp
class Component abstract { //컴포넌트 인터페이스
public:
	virtual void Notify(std::string message) = 0; //메세지를 받는 알림 함수입니다.
protected:
	Object* _object = nullptr; //본인을 가지고 있는 오브젝트(여기선 Player가 되겠습니다.)
};
```
컴포넌트 클래스에도 알림을 정의합니다.<br>
서브 클래스들이 재정의 하여 본인의 알림을 처리합니다.
```cpp
class Transform final : public Component { //위치 컴포넌트
public:
	virtual void Notify(std::string message) override { //알림 함수 재정의
		if ("setpos" == message) //메세지가 setpos라면
			SetPos(); //SetPos를 호출합니다.
	}
	void SetPos(void) {
	}
};
```
```cpp
class Collider final : public Component { //물리 컴포넌트
public:
	virtual void Notify(std::string message) override {
	}
	void IsCollision(void) {
		if (true) //만약 충돌 했다면
			_object->notify("setpos"); //오브젝트 알림 함수로 메세지를 보냅니다.
	}
};
```
Collider 컴포넌트에서 Object로 알림을 보내고<br>
Object 클래스에서 모든 Component로 알림을 보냅니다.<br><br>
알림을 받은 Transform은 그에 맞는 처리를 진행합니다.<br>
<br>
이번 예제에서는 string 형식으로 메세지롤 보냈지만<br>
메세지를 좀더 디테일하게 보내기 위해서 Command 패턴을 사용하는 것도 좋은 방법입니다.<br>
<br>
세 방식 모두 장단점이 존재하기 때문에<br>
상황에 맞춰 적당한 방법을 선택하는것이 좋습니다.
