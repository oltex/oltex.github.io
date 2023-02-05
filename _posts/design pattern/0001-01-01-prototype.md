---
title: "원형(Prototype)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

생성 디자인 패턴중 하나인 프로토타입 패턴입니다.
원형이 되는 클래스의 인스턴스를 사용하여 생성할 객체의 종류를 명시하고
이렇게 만든 견본을 복사하여 새로운 객체를 생성하는 패턴입니다.
> ## 필요성

게임을 만들고자 할 때 고블린 몬스터를 생성하기위해 스폰 클래스를 만들고,
그 안에 팩토리 메소드가 존재한다고 가정해보겠습니다.
고블린 몬스터는 검을든 고블린 또는 활을든 고블린 방패를든 고블린등 장비가 다양하게 존재할 수 있습니다.

이러한 상황에서 고블린을 스폰하고 매번 장비를 쥐어주거나, 검 고블린, 활 고블린등 클래스를 나누는 작업은
번거롭고 코드량과 클래스가 많아지게 됩니다.
```cpp
class Goblin abstract {
};
class SwordGoblin final : public Goblin {
};
class BowGoblin final : public Goblin {
};
class ShieldGoblin final : public Goblin {
};

class Spawn abstract {
	virtual Goblin* FactoryMethod(void) = 0;
};
class SwordGoblinSpawn final : public Spawn {
	virtual Goblin* FactoryMethod(void) override {
		return new SwordGoblin;
	};
};
class BowGoblinSpawn final : public Spawn {
	virtual Goblin* FactoryMethod(void) override {
		return new BowGoblin;
	};
};
class ShieldGoblinSpawn final : public Spawn {
	virtual Goblin* FactoryMethod(void) override {
		return new ShieldGoblin;
	};
};
```
이러한 상황에서 쓸 수 있는 패턴이 프로토타입 패턴입니다.
고블린 클래스 하나만 존재하게 만들고 미리 프로토타입이 될 객체들에게 각각 무기를 쥐어준 후
그들을 복제해서 사용한다면 클래스의 양을 늘리지 않고 다양한 타입의 객체들을 생성할 수 있습니다.
이를 좀더 범용적으로 사용한다면 모든 몬스터들에 대한 프로토타입 스포너를 만들수도 있습니다.

프로토 타입 패턴은 복사를 통해 객체 생성이 이루어집니다.
때문에 만약 객체의 생성과정이 복잡하고 리소스를 많이 먹는다면 또는 객체의 생성이 지속적으로 이루어져야 한다면
프로토 타입을 사용해 성능 향상을 꾀할 수 있습니다.

프로토 타입 패턴은 원형 클래스를 생성하는 과정이 존재하고 이는 런타임에 이루어지기 때문에
동적으로 클래스를 확장할 수 있다는 이점이 존재합니다.

> ## 구현

앞서 얘기한 고블린을 예로들어서 구현하면 이렇습니다.
```cpp
class Goblin final {
public:
	Goblin* Clone(void) { //프로토타입에 등록될 클래스는 클론 함수가 필요합니다.
		return  new Goblin{*this}; //자신을 복사 생성
	}
};

class Prototype final {
public:
	void Add(const std::string& key, Goblin* goblin) { //프로토 타입에 등록합니다.
		_prototype.emplace(key, goblin);
	}
	Goblin* Clone(const std::string& key) { //등록된 프로토 타입으로 부터 클론합니다.
		return _prototype[key]->Clone();
	}
private:
	std::map<std::string, Goblin*> _prototype;
};

void main(void) {
	Goblin* swordGoblinPrototype = new Goblin;
	Goblin* bowGoblinPrototype = new Goblin;
	Goblin* shieldGoblinPrototype = new Goblin;
	//프로토 타입 고블린들을 상태에 맞게 초기화합니다.
  //각각 검, 활, 방패를 들게합니다.
	
	Prototype* prototype = new Prototype; //고블린들을 프로토 타입에 등록합니다.
	prototype->Add("SwordGoblin", swordGoblinPrototype);
	prototype->Add("BowGoblin", bowGoblinPrototype);
	prototype->Add("ShieldGoblin", shieldGoblinPrototype);

	//고블린들을 클론합니다.
	Goblin* swordGoblinClone = prototype->Clone("SwordGoblin");
	Goblin* bowGoblinClone = prototype->Clone("BowGoblin");
	Goblin* shieldGoblinClone = prototype->Clone("ShieldGoblin");
}
```
이러한 방식으로 프로토 타입을 구현하면 고블린 클래스가 하나만 존재하면 되고
스폰을 담당하는 클래스도 한개만 존재하면 충분해집니다.<br>
---
프로토타입을 사용한 예제로 고블린만을 들었지만
각기 다른 클래스라도 인터페이스는 동일하다면 프로토 타입 패턴을 사용할 수 있습니다.

팩토리 메소드나 추상 클래스를 사용하면 어찌 됐든 그만큼 함수와 코드의 양이 많아진다는 단점이 존재합니다.(템플릿 제외)
허나 프로토타입은 원형을 추가해놓기만 한다면 같은 인터페이스를 가진 모든 클래스의 생성을 지원한다는 점에서 이점이 있습니다.

예를 들어 몬스터의 하위 클래스로 슬라임 박쥐 고블린이 존재한다고 가정했을 때
팩토리 메소드는 이를 각각 생성하는 함수가 총 3개
추상 팩토리는 이를 담당하는 클래스가 1개 함수가 3개 존재해야 합니다.
혹은 매개 변수를 받아서 조건문을 통해 정적으로 코딩해야합니다.
(템플릿을 사용하면 문제없습니다.)

하지만 프로토타입을 사용하면 이를 해결할 수 있습니다.
위쪽 예시를 프로토타입으로 구현한 코드입니다.
```cpp
class Monster abstract {
public:
	virtual Monster* Clone(void) = 0;
};
class Slime final : public Monster {
public:
	virtual Monster* Clone(void) override {
		return new Slime{ *this };
	};
};
class Bat final : public Monster {
public:
	virtual Monster* Clone(void) override {
		return new Bat{ *this };
	};
};
class Goblin final : public Monster {
public:
	virtual Monster* Clone(void) override {
		return new Goblin{ *this };
	};
};

class Prototype final {
public:
	void Add(const std::string& key, Monster* monster) {
		_prototype.emplace(key, monster);
	}
	Monster* Clone(const std::string& key) {
		return _prototype[key]->Clone();
	}
private:
	std::map<std::string, Monster*> _prototype;
};
```
고블린 코드랑 다를게 없긴하지만 몬스터 클래스의 확장성 면에서 유리합니다.
만약 몬스터 클래스를 추가하여도 프토토 타입 클래스를 변경할 필요가 없어집니다.

만약 추상 팩토리도 이와 비슷하게 만들고 싶다면 템플릿을 사용하면 비슷하게 만들 수 있습니다.
