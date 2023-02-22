---
title: "책임 연쇄(Chain of Responsibility)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

행동 디자인 패턴중 하나인 책임 연쇄 패턴입니다.<br>
하나의 요청에 대한 처리를 한 객체가 담당하지 않고 여러 객체에게 처리의 기회를 주는 패턴입니다.<br>
<br>
요청이 들어오면 각 객체는 체인을 따라 요청을 처리 혹은 전달할지 결정합니다.
> 필요성

주문 시스템을 만들고 있다고 합니다.<br>
요청이 들어오고 이를 승인하기 전에 여러가지 검사/처리를 거쳐야 한다고 합니다.<br>
<br>
접속: 사용자 계정이 로그인 되어있는지 검사합니다.<br>
권한: 이 상품을 주문할 권한이 있는지 확인합니다.<br>
확인: 주문 내역이 유효한지 확인합니다.<br>
실드: 외부에서 온 악의적인 공격이 아닌지 확인합니다.<br>
캐시: 반복된 데이터 요청이라면 성능을 향상시킵니다.<br>
<br>
각 처리는 복잡한 절차를 거쳐야 합니다.<br>
(간단하게 만들기 위해 접속과 권한만 처리해보겠습니다.)
```cpp
class Login final {
public:
	bool process(void) {
		return true;
	}
};
class Power {
public:
	bool process(void) {
		return true;
	}
};

void main(void) {
	Login login;
	Power power;

	//request 요청

	if (false == login.process()) //검사
		return;
	if (false == power.process()) //검사
		return;

	//approval 승인
}
```
요청과 승인 코드 주석 사이에 두 가지 검사가 존재하게 되었습니다.
만약 모든 처리에 대해 작성한다면 코드가 길어져
메인 함수와 처리의 커플링이 증가해 수정하기 어려워질 것입니다.<br>
<br>
또한 이러한 검사코드를 다른 파트에도 적용하려 한다면 코드를 복제해야 할 것입니다.
> ## 구현

책임 연쇄 패턴을 사용하면 각 처리들을 핸들러라는 객체로 변환합니다.<br>
각 핸들러에는 다음 핸들러를 위한 포인터가 존재합니다.<br>
<br>
(코드를 간결하게 하기 위해 가상 소멸자, delete를 제외하였습니다.)
```cpp
class Handler abstract { //핸들러 입니다.
public:
	Handler(Handler* handler) :
		_handler(handler) {
	}
	virtual bool process(void) = 0;
protected:
	Handler* _handler = nullptr; //다음 핸들을 가리키는 포인터입니다.
};

class Login final : public Handler { //핸들러를 상속받는 처리 클래스 입니다.
public:
	Login(Handler* handler) : //각 처리들은 생성자에서 다음 핸들을 받을 수 있습니다.
		Handler(handler) {
	}
	virtual bool process(void) override { //프로세스 함수가 성공적으로 통과되면 다음 핸들에 요청을 전달합니다.
		if (true) {
			if (nullptr == _handler)
				return true;
			else
				return _handler->process();
		}
		else
			return false;
	}
};
class Power final : public Handler {
public:
	Power(Handler* handler) :
		Handler(handler) {
	}
	virtual bool process(void) override {
		if (true) {
			if (nullptr == _handler)
				return true;
			else
				return _handler->process();
		}
		else
			return false;
	}
};
class Check final : public Handler {
public:
	Check(Handler* handler) :
		Handler(handler) {
	}
	virtual bool process(void) override {
		if (true) {
			if (nullptr == _handler)
				return true;
			else
				return _handler->process();
		}
		else
			return false;
	}
};
class Shield final : public Handler {
public:
	Shield(Handler* handler) :
		Handler(handler) {
	}
	virtual bool process(void) override {
		if (true) {
			if (nullptr == _handler)
				return true;
			else
				return _handler->process();
		}
		else
			return false;
	}
};
class Cache final : public Handler {
public:
	Cache(Handler* handler) :
		Handler(handler) {
	}
	virtual bool process(void) override {
		if (true) {
			if (nullptr == _handler)
				return true;
			else
				return _handler->process();
		}
		else
			return false;
	}
};

void main(void) {
	Handler* handler = new Login{ new Power{new Check{ new Shield{new Check{nullptr}}}}};

	//request

	if (false == handler->process())
		return;

	//approval
}
```
이제 메인 함수와 각 처리들간의 커플링을 방지할 수 있습니다.<br>
책임 연쇄 패턴의 장점은 각 처리들의 결합도를 낮추고 동적으로 결합할 수 있다는 점입니다.<br>
<br>
예를 들어 위 코드에서 캐시가 필요 없다면 캐시를 체인에서 제외하기만 하면 될 것입니다.
