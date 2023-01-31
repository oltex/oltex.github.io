---
title: "팩토리 메서드(Factory Method)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

생성 패턴중 하나인 팩토리 메서드 패턴입니다.<br>
부모 클래스에서 객체의 생성을 위한 인터페이스를 정의하고 자식 클래스가 정의한 인터페이스를 구현하는 디자인 패턴입니다.<br>
다시 말해 객체를 생성하는 과정을 직접 수행하지 않고 대행하는 함수를 두어 작업을 맏기는 방식입니다.<br>
<br>
팩토리 메서드를 사용하면 객체를 생성하는 과정이 복잡할 때 코드간의 결합도를 낮춰 유용하게 대처할 수 있습니다.<br>
또한 객체의 생성 기능 변경이나 객체의 추가등 유지관리면에서 수월해집니다.<br>
<br>
(단순히 객체를 생성하는 메서드는 팩토리 메서드가 아니라고 합니다. 팩토리 메서드를 사용하는 중요한 키워드는 상속과 가상 함수입니다.<br>
템플릿 메서드의 생성 패턴 버전이라고 볼 수 있다합니다.)
> ## 구현

객채를 생성하기 위해 우리는 보통 new 키워드를 사용해왔습니다.<br>
아래 코드는 몬스터 클래스의 자식 클래스인 박쥐와 거미를 생성하는 코드입니다.
```cpp
class Monster abstract {
};
class Bat final : public Monster {
};
class Spider final : public Monster {
};
void main(void) {
	Monster* bat = new Bat;
	Monster* spider = new Spider;
	delete bat;
	delete spider;
}
```
이 코드를 팩토리 메서드를 사용하여 변경 해 보겠습니다.
```cpp
class Monster abstract {
};
class Bat final : public Monster {
};
class Spider final : public Monster {
};
class Creator abstract {
public:
	virtual Monster* FactoryMethod(void) = 0; //팩토리 메서드 구현
};
class BatCreator final : public Creator {
public:
	virtual Monster* FactoryMethod(void) override {
		return new Bat;
	};
};
class SpiderCreator final : public Creator {
public:
	virtual Monster* FactoryMethod(void) override {
		return new Spider;
	}
};
void main(void) {
	BatCreator batCreator;
	Monster* bat = batCreator.FactoryMethod();
	BatCreator spiderCreator;
	Monster* spider = spiderCreator.FactoryMethod();
	delete bat;
	delete spider;
}
```
Creator 클래스를 구현하고 그안에 FactoryMethod 함수를 만들었습니다.<br>
그 다음 Creator를 상속받은 BatCreator와 SpiderCreator 두 클래스에서 각각<br>
Bat과 Spider를 생성할 수 있도록 재정의 했습니다.
> ## 필요성
언뜻 보면 이게 필요가 있나 싶어지는 구조입니다. 아래 예제는 팩토리 메드가 필요한 경우의 예시를 들어보았습니다.
### 생성이 복잡 할 경우
만약 각 객체의 생성이 복잡하다면 어떻게 될까요?
```cpp
class Monster abstract {
};
class Bat final : public Monster {
};
class Spider final : public Monster {
};
void main(void) {
	// bat 객체 풀 검사
	Monster* bat = new Bat;
	// bat 널 포인터 검사
	// bat 초기화 
	// bat 기타 복잡한 생성 과정...
	// ...
	// ...

	// spider 객체 풀 검사
	Monster* spider = new Spider;
	// spider 널 포인터 검사
	// spider 초기화
	// spider 기타 복잡한 생성 과정...
	// ...
	// ...

	delete bat;
	delete spider;
}
```
main코드가 담당하는 부분이 많아져 결합도가 높아졌습니다.<br>
만약 monster와 spider클래스를 생성하는 코드가 main함수가 아니라 다른 곳에도 작성 돼야 한다면 코드를 중복해서 사용해버리게 됩니다.<br>
혹은 main의 코드가 길어지고 그 후 bat 코드의 생성 로직이 수정되어서 생성방식을 바꿔야한다면 위치를 찾기 힘들 수 있습니다.<br>
위의 이유로 코드의 유지보수가 힘들어질 것입니다.<br>
 <br>
이러한 경우 팩토리 메서드를 사용하면 코드를 간결하게 유지할 수 있습니다.
```cpp
class Monster abstract {
};
class Bat final : public Monster {
};
class Spider final : public Monster {
};
class Creator abstract {
public:
	virtual Monster* FactoryMethod(void) = 0;
};
class BatCreator final : public Creator {
public:
	virtual Monster* FactoryMethod(void) override {
		// bat 객체 풀 검사
		Monster* bat = new Bat;
		// bat 널 포인터 검사
		// bat 초기화 
		// bat 기타 복잡한 생성 과정...
		// ...
		// ...
		return bat;
	};
};
class SpiderCreator final : public Creator {
public:
	virtual Monster* FactoryMethod(void) override {
		// spider 객체 풀 검사
		Monster* spider = new Spider;
		// spider 널 포인터 검사
		// spider 초기화
		// spider 기타 복잡한 생성 과정...
		// ...
		// ...
		return spider;
	}
};
void main(void) {
	BatCreator batCreator;
	Monster* bat = batCreator.FactoryMethod();
	BatCreator spiderCreator;
	Monster* spider = spiderCreator.FactoryMethod();
	delete bat;
	delete spider;
}
```
main함수에서 사용되는 코드는 변하지 않았다는 것을 볼 수 있습니다.<br>
만약 bat 코드의 생성 로직이 수정되더라도 batCreator안에 있는 팩토리 메서드만 수정해주면 되는것입니다.<br>
또한 다른 곳에서 생성을 사용한다 하더라도 코드의 중복을 최소화 할 수 있습니다.
> ## 주의점

### 추상 팩토리
위 코드는 Bat과 Spider몬스터를 생성하기 위해 Creator라는 클래스를 정의하여 사용하였습니다.<br>
<br>
팩토리에 대해 공부하다가 추상 팩토리 패턴을 알고<br>
팩토리 패턴의 차이점에 대해 굉장히 고민했습니다.<br>
추상 팩토리도 클래스를 만들고 그안에서 함수를 만들어서 객체를 반환하는지라<br>
추상팩토리랑 다른게 뭔데? 라는 의문을 가졌었습니다.<br>
<br>
한참동안 고민하다가 내린 정의는<br>
팩토리 메서드의 주된 개념은 메서드라는 점을 놓치면 안된다 라는 것입니다.<br>
즉 Creator라는 클래스를 생성하는것이 팩토리 메서드가 아니라,<br>
FactoryMethod라는 함수를 선언하는것이 팩토리 메서드라는 것이죠.<br>
(명확하게 하기위해 FactoryMethod라는 함수 이름을 사용했습니다.)<br>
<br>
이와 반대로 추상 팩토리의 주된 개념은 객체이고<br>
Creator를 생성하는것이 주된 목적입니다.<br>
(추상클래스에서는 Creator를 AbstractFactory라고 명명합니다.)<br>
<br>
예시를 들기위해 Creator를 Dungeon으로 바꿔보겠습니다.
```cpp
class Dungeon abstract {
public:
	void Enter(void) { };
	void Exit(void) { };
	void Clear(void) { };
	void Fail(void) { };
	void Spawn(void) {
		m_vecMonster.emplace_back(FactoryMethod());
	}
private:
	virtual Monster* FactoryMethod(void) = 0;
private:
	std::vector<Monster*> m_vecMonster;
};
class BatDungeon final {
private:
	virtual Monster* FactoryMethod(void) {
		return new Bat;
	}
};
class SpiderDungeon final {
private:
	virtual Monster* FactoryMethod(void) {
		return new Spider;
	}
};
```
던전 클래스에 여러가지 기능이 추가되었습니다.<br>
던전에 입장하기, 나가기, 성공, 실패 등 던전에는 다양한 함수들이 필요할 것입니다.<br>
그 중 스폰 함수는 몬스터를 스폰하는 함수입니다.<br>
던전에서 생성되는 몬스터가 한 종류라고 가정하고 각 던전마다 스폰되는 몬스터가 다르다고 할 때<br>
팩토리 메서드를 사용하여 각 던전에서 생성되는 몬스터를 다르게 설정하였습니다.<br>
<br>
이 코드의 핵심은 Dungeon클래스는 오롯이 Monster를 생성하기 위한 클래스가 아니라는 것입니다.<br>
던전 클래스에서 다른 던전마다 생성되는 몬스터를 다르게 하기위한 방식이 필요했고<br>
그 방법으로 팩토리 메서드 함수를 선택했습니다.<br>
<br>
즉,
1. Monster를 스폰하기 위해 Dungeon 클래스를 만든것이 아니라
2. Dungeon클래스가 필요에 의해 Monster를 다르게 스폰했다는것입니다.

허나 1번의 방법이 팩토리 메서드가 아닌것 또한 아닙니다.<br>
단지 2번은 1번의 방법만이 팩토리 메서드를 사용하는 방법이 아니라는 하나의 반례일 뿐이니(또는 팩토리 메서드는 객체가 아니라 함수가 중심이라는 의미)<br>
1번과 2번 둘다 팩토리 메서드를 사용한것이 맞다는 점을 유의해야합니다.<br>
<br>
추상 팩토리의 의미는 1번에 좀 더 가깝습니다. (완전히 같은건 아닙니다.)<br>
여러가지 몬스터들이 있고 그 몬스터들을 생성할 수 있는 객체가 있으면 좋겠다 라는 뜻에서 만들어진 것입니다.<br>
이러한 점에서 추상 팩토리는 팩토리 메서드의 부분집합인 것입니다.
> ## 사용법

팩토리 메서드의 여러가지 사용방법에 대해 서술하겠습니다.
### 매개변수
팩토리 메서드는 매개변수를 사용하여 다양한 형태의 객체를 생성하게 만들 수 있습니다.<br>
(날 수 있는 몬스터만 나오는 던전을 생성하자)
```cpp
class FlyDungeon final : public Dungeon {
public:
	virtual Monster* FactoryMethod(const int& index) override {
		Monster* monster = nullptr;
		switch (index) {
		case 0: 
			monster = new Bat;
			break;
		case 1:
			monster = new Bird;
		}
		return monster;
	};
};
```
### 템플릿
앞선 코드에서 몬스터의 종류가 추가될 때마다 크리에이터 클래스도 계속 추가 서브클래싱 되어야 했습니다.<br>
허나 템플릿 코드를 사용하면 이를 종류에 따라 좀더 간결하게 유지할 수 있습니다.<br>
(예를들어 땅에서 행동하는 몬스터는 초기화가 전부 동일하다 라고 한다면)
```cpp
class Creator abstract {
public:
	virtual Monster* FactoryMethod(void) = 0;
};
template<typename T>
class GroundCreator final : public Creator {
public:
	virtual Monster* FactoryMethod(void) override {
		return new T;
	};
};
```
