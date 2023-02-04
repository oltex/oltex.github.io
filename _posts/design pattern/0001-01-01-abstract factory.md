---
title: "추상 팩토리(Abstract Factory)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

생성 패턴중 하나인 추상 팩토리 패턴입니다.<br>
추상 팩토리 패턴은 팩토리 메서드를 이용하여 만들어지기 때문에 팩토리 메서드를 이해하고 있는 편이 좋습니다.<br>
<br>
객체의 생성을 제어하는 패턴으로 팩토리 메서드를 사용하려할 때<br>
서로 관련있는 클래스들을 생성할 수 있는 클래스를 만들고 싶을 때 사용합니다.<br>
추가로 각 클래스들의 다른 버전들이 존재하고 그들을 대체 시키는 작업 또한 가능합니다.<br>
<br>
예를 들어 던전에서 생성되는 모든 오브젝트를 관리하고 싶다고 가정해봅시다.
그 오브젝트들의 종류로는 몬스터와 아이템이 존재하고 이를 생성하는 클래스로 Spawn 추상 팩토리를 만들수 있습니다.<br>
또한 스폰 클래스가 더 강한 몬스터와 아이템을 위한 Hard스포너와 약한 몬스터와 아이템을 위한 Easy스포너로 나누고<br>
두 스포너가 서로 대체할 수 있게 만들 수 있습니다.
> ## 구현

추상 팩토리의 구현입니다. 앞서 예시를 들었던 클래스로 작성해보겠습니다.<br>
(스폰 클래스는 AbstractFactory라는 이름으로 작성하겠습니다.)
```cpp
class Monster abstract { };
class NormalMonster final : public Monster { };
class Item abstract { };
class NormalItem final : public Item { };
class AbstractFactory final { //추상 팩토리
public:
	Monster* SpawnMonster(void) { //Monster 팩토리 메서드
		return new NormalMonster;
	}
	Item* SpawnItem(void) { //Item 팩토리 메서드
		return new NormalItem;
	}
};
void main(void) {
	AbstractFactory abstractFactory;
	Monster* normalMonster = abstractFactory.SpawnMonster();
	Item* normalItem = abstractFactory.SpawnItem();
	delete normalMonster;
	delete normalItem;
}
```
추상 팩토리 객체를 만들고 그를 통해서 NormalMonster과 NormalItem 객체를 생성했습니다.<br>
여기서 추상 팩토리가 추상 클래스가 아니라는 점이 의아할 수 있는데 추상 클래스를 응용하는 또 다른 일반적인 구현 방법이라고 보시면 됩니다.<br>
<br>
이 코드에서 팩토리 메서드가 총 2개 존재합니다.(엄밀히 말하면 상속을 하지 않았기 때문에 팩토리 메서드가 아니긴합니다.)<br>
그리고 각 팩토리 메서드가 담당하는 추상클래스도 2개 존재합니다.<br>
이를 묶어서 추상 팩토리화 시켰다고 보시면 되겠습니다.<br>
<br>
이번에는 예시를 바꿔서 Easy와 Hard스포너(AbstractFactory) 클래스를 제작해보겠습니다.
```cpp
class Monster abstract { }; //몬스터: 이지, 노말, 하드
class EasyMonster final : public Monster { };
class NormalMonster final : public Monster { };
class HardMonster final : public Monster { };
class Item abstract { }; //아이템: 이지, 노말, 하드
class EasyItem final : public Item { };
class NormalItem final : public Item { };
class HardItem final : public Item { };
class AbstractFactory abstract { //추상 팩토리
public:
	virtual Monster* SpawnMonster(void) = 0;
	virtual Item* SpawnItem(void) = 0;
};
class EasyAbstractFactory final { //이지 추상 팩토리
public:
	virtual Monster* SpawnMonster(void) {
		return new EasyMonster;
	}
	virtual Item* SpawnItem(void) {
		return new EasyItem;
	}
};
class HardAbstractFactory final { //하드 추상 팩토리
public:
	virtual Monster* SpawnMonster(void) {
		return new HardMonster;
	}
	virtual Item* SpawnItem(void) {
		return new HardItem;
	}
};
void main(void) {
	EasyAbstractFactory easyAbstractFactory;
	Monster* easyMonster = easyAbstractFactory.SpawnMonster();
	Item* easylItem = easyAbstractFactory.SpawnItem();
	delete easyMonster;
	delete easylItem;

	HardAbstractFactory hardAbstractFactory;
	Monster* hardMonster = hardAbstractFactory.SpawnMonster();
	Item* hardlItem = hardAbstractFactory.SpawnItem();
	delete hardMonster;
	delete hardlItem;
}
```
이제 정말로 추상 팩토리답게 추상화가 되었습니다.
> ## 특징

구상 클래스에 의존하지 않고 서로 관련된 객체들의 생성을 한데 모아주는 역할을 합니다.
