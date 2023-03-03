
---
title: "하위 클래스 샌드박스(Subclass Sandbox)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

하위 클래스 샌드박스 디자인 패턴입니다.<br>
상위 클래스가 제공하는 기능들을 통해서 하위 클래스의 행동을 제작합니다.
> ## 필요성

클래스를 제작하다 보면 종종 상위 클래스로 기능을 합치는<br>
경우가 존재합니다.<br>
<br>
예를 들어 초능력 클래스를 제작한다고 가정해보겠습니다.<br>
초능력으로는 여러 가지가 존재할 수 있습니다.<br>
<br>
초능력 클래스로 Superman, Batman, Ironman을 만들었습니다.<br>
(초능력을 구상하기 애매해서 초능력자 이름을 사용했습니다.<br>
이 초능력자들 이름에 맞는 초능력 클래스라고 생각해주세요.)<br>
<br>
이 들은 초능력이라는 하나의 공통된 개념이 존재하기 때문에<br>
Superpower상위 클래스를 만들고 상속 관계를 형성할 수 있을 것입니다.
```cpp
class Superpower abstract {
	virtual void Action(void) = 0;
};
```
```cpp
class Ironman final : public Superpower {
public:
	virtual void Action(void) override {
		//레이저 빔을 쏩니다.
		//소리가 납니다.
		//파티클이 튑니다.
	};
};
class Hulk final :public Superpower {
public:
	virtual void Action(void) override {
		//분노를 표출합니다.
		//소리가 납니다.
		//파티클이 튑니다.
	};
};
class Thor final : public Superpower {
public:
	virtual void Action(void) override {
		//망치를 던집니다.
		//소리가 납니다.
		//파티클이 튑니다.
	};
};
```
초능력 클래스에 초능력을 사용할 수 있는 Action함수를 정의했습니다.<br>
이제 각 초능력에 맞는 Action을 재정의 한다고 합니다.<br>
<br>
각 초능력 Action 함수에는 공통적으로 들어가는 코드가 존재합니다.<br>
이로 인해 몇가지 문제가 발생할 수 있습니다.
- 중복 코드가 많아집니다: 코드가 중복되는 상황을 볼 수 있습니다.
- 불변식을 지키지 어렵습니다: 사운드를 출력하는 코드는<br>
특정한 규칙에 따라 호출될 수 있습니다. 이를 서브 클래스에서 정의한다면<br>
달라질 가능성이 존재합니다.
- 외부 시스템이 변경될 가능성이 있습니다: 사운드를 출력하는 함수는 컴포넌트를<br>
사용하여 구현했다고 가정해보겠습니다. PlaySound 함수를 호출하도록 이루어졌는데<br>
나중가서 함수의 이름이 SoundPlay로 변경된다면 모든 초능력 클래스를 수정해야합니다.
- 커플링이 심해집니다: 위 예시랑 비슷합니다. 커플링은 지양해야 되는데<br>
모든 초능력 클래스가 Sound 컴포넌트랑 커플링됩니다.
> ## 구현

이러한 문제를 해결하기 위해
