---
title: "타입 객체"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

타입 객체 디자인 패턴입니다.<br>
클래스 하나를 인스턴스 별로 나누어 다른 타입으로 표현할수 있게 만듭니다.<br>
새로운 클래스들을 유연하게 만들 수 있게 합니다.<br>
<br>
클래스를 분리하기 위해 상속을 사용하는 대신, 클래스의 인스턴스를 타입별로 생성하고,<br>
객체 합성을 통해 이를 사용합니다.
> ## 필요성

하나의 개념이 구체적인 개념으로 나뉠 때 대부분 상속을 통해 이를 해결합니다.<br>
허나 이 방식은 충분한 융통성을 제공하지 않는 경우가 존재합니다.<br>
<br>
제공하는 클래스의 양이 많아질 수록 이를 수정 확장하기 쉽지 않다는 점이<br>
그 중 하나입니다.<br>
<br>
예를 들어 보겠습니다.<br>
<br>
게임을 제작하던 중<br>
Monster 클래스를 만들고 이를 상속받는 Dragon과 Slime 클래스를 만들었다고 합니다.<br>
모든 몬스터에는 각 몬스터의 공격력(attack)과 체력(Hp)이 존재합니다.<br>
<br>
이제 각 종족에 맞게 attack과 hp를 조정해줘야 합니다.
```cpp
class Monster abstract {
protected:
	int _attack = 0;
	int _hp = 0;
};
```
```cpp
class Dragon final : public Monster {
	Dragon(void) {
		_attack = 20;
		_hp = 20;
	}
};
```
```cpp
class Slime final : public Monster {
public:
	Slime(void) {
		_attack = 10;
		_hp = 10;
	}
};
```
성공적으로 잘 조정된것 같습니다.<br>
허나 시간이 지나고난 후 문제가 발생하게 됩니다.<br>
```cpp
class Other final : public Monster {
public:
	Other(void) {
		_attack = 10;
		_hp = 10;
	}
};
```
```cpp
... //또 다른 몬스터
```
```cpp
... //또또 다른 몬스터
```
```cpp
... //또또또 다른 몬스터
```
수백종의 몬스터를 만드는것이 목적이다보니,<br>
사용자는 몇 줄 안되는 하위 클래스를 하루종일 작성하고 컴파일 하게 됩니다.<br>
<br>
이미 만들어진 코드를 변경하려 할 때도 문제가 발생합니다.<br>
컴파일 타임에 몬스터의 공격력과 체력을 정의하다 보니 체력을 변경해달라는 요청이<br>
들어오게 되면 변경 후 컴파일을 또 진행해야됩니다.
> ## 구현

이러한 상황을 타입 객체 패턴은<br>
타입의 생성을 컴파일 타임에서, 런 타임으로 옮김으로써 해결하려 합니다.<br>
<br>
타입 객체 패턴은 다음과 같은 상황에 유용합니다.
- 생성해야될 타입의 종류가 많을 때
- 나중에 어떤 타입이 만들어 질지 모르는 상황에, 컴파일/코드의 추가 없이 타입을 추가/변경하려할 때

위 코드를 변경해 보겠습니다.
```cpp
class Type final { //타입 객체입니다. 몬스터의 타입을 정의합니다.
public:
	Type(std::string name, int attack, int hp) :
		_name(name), _attack(attack), _hp(hp) {
	}
	int GetHp(void) {
		return _hp;
	}
private:
	std::string _name = ""; //타입에 정의된 종족의 이름(dragon, slime, ...)
	int _attack = 0; //타입에 정의된 종족의 공격력
	int _hp = 0; //타입에 정의된 종족의 최대 체력
};
```
```cpp
class Monster final {
public:
	Monster(Type& type) : //몬스터를 생성할 때 타입을 받습니다.
		_type(type),
		_hp(type.GetHp()) { //최대 체력을 받아, 현재 체력을 입력합니다.
	}
protected:
	Type& _type;
	int _hp = 0;
};
```
```cpp
void main(void) {
	Type dragon{ "dragon", 20, 20 }; //타입을 정의합니다.
	Type slime{ "slime", 10, 10 };

	std::vector<Monster*> monsters;
	monsters.emplace_back(new Monster{ dragon }); //이제 몬스터를 생성할 때 타입을 사용합니다.
	monsters.emplace_back(new Monster{ slime });
};
```

