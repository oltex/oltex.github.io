---
title: "팩토리 메소드(Factory Method)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

생성 패턴중 하나인 팩토리 메소드 패턴입니다.
부모 클래스에서 객체의 생성을 위한 인터페이스를 정의하고 자식 클래스가 정의한 인터페이스를 구현하는 디자인 패턴입니다.
다시 말해 객체를 생성하는 과정을 직접 수행하지 않고 대행하는 함수를 두어 작업을 맏기는 방식입니다.

팩토리 메소드를 사용하면 객체를 생성하는 과정이 복잡할 때 코드간의 결합도를 낮춰 유용하게 대처할 수 있습니다.
또한 객체의 생성 기능 변경이나 객체의 추가등 유지관리면에서 수월해집니다.

(단순히 객체를 생성하는 메소드는 팩토리 메소드가 아니라고 합니다. 팩토리 메소드를 사용하는 중요한 키워드는 상속과 가상 함수입니다.<br>
템플릿 메소드의 생성 패턴 버전이라고 볼 수 있다합니다.)
> ## 필요성

객채를 생성하기 위해 우리는 보통 new 키워드를 사용해왔습니다.
아래 코드는 몬스터 클래스의 자식 클래스인 박쥐와 거미를 생성하는 코드입니다.
```cpp
class Monster {
};
class Bat : public Monster {
};
class Spider : public Monster {
};
void main(void) {
	Monster* bat = new Bat;
	Monster* spider = new Spider;
	delete bat;
	delete spider;
}
```
이 코드를 팩토리 메소드를 사용하여 변경 해 보겠습니다.
```cpp
class Monster abstract {
};
class Bat final : public Monster {
};
class Spider final : public Monster {
};
class Creator abstract {
public:
	virtual Monster* FactoryMethod(void) = 0; //팩토리 메소드 구현
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
Creator 클래스를 구현하고 그안에 FactoryMethod 함수를 만들었습니다.
그 다음 Creator를 상속받은 두 클래스에서 각각 재정의 시켰습니다.
언뜻 보면 이게 필요가 있나 싶어지는 구조입니다. 이 경우 팩토리 클래스를 

> ## 정적 팩토리 메소드
