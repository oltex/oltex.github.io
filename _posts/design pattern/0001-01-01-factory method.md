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
> ## 구현

객채를 생성하기 위해 우리는 보통 new 키워드를 사용해왔습니다.
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

> ## 필요성
언뜻 보면 이게 필요가 있나 싶어지는 구조입니다. 아래 예제는 팩토리 메소드가 필요한 경우의 예시를 들어보았습니다.
### 생성과정이 복잡 할 경우
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
이렇게 되면 main코드의 결합도가 높아져서 유지보수가 힘든 코드가 돼 버렸습니다.
만약 monster와 spider클래스를 생성하는 코드가 main함수가 아니라 다른 곳에도 작성 돼야 한다면,
혹은 bat 코드의 생성 로직이 수정되어서 생성방식을 바꿔야한다면,
코드 유지관리 측면에서 굉장히 힘들 수 있습니다.

이러한 경우 팩토리 메소드를 사용하면 코드를 간결하게 유지할 수 있습니다.
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
main함수에서 사용되는 코드는 변하지 않았다는 것을 볼 수 있습니다.
만약 bat 코드의 생성 로직이 수정되더라도 batCreator안에 있는 팩토리 메소드만 수정해주면 되는것입니다.

### 
> ## 정적 팩토리 메소드
