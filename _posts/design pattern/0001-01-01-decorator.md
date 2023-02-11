---
title: "장식자(Decorator)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

구조 디자인 패턴 중 하나인 장식자 패턴입니다.
객체에 동적으로 새로운 행동을 추가할 수 있게 만듭니다.

새로운 행동들을 포함한 장식자 객체를 추가하고 기존 객체와 연결하여 해당 행동을
연결시키는 패턴입니다.
> ## 필요성

장식자 패턴은 다음과 같은 상황에서 사용하면 좋습니다.
- 동적으로 각각의 객체에 기능을 추가해야합니다.
- 또한 동적으로 제거될 수 있는 기능입니다.

화면에 창을 출력하기 위해 Window 클래스를 제작하였습니다.
윈도우 클래스에는 화면을 출력하기 위한 View함수가 존재합니다.

윈도우 클래스의 구체 클래스로 TextWindow와 DocWindow가 존재합니다.
```cpp
class Window abstract {
public:
	virtual void View(void) = 0;
};

class TextWindow final : public Window {
};
class DocWindow final : public Window {
};
```
이후 특정 윈도우 클래스 객체는
두꺼운 테두리를 추가하거나 스크롤을 추가해줄 필요가 존재해집니다.
(두 가지 기능을 전부 가지고 있는 객체도 존재하지만 코드가 너무 길어져서 제외했습니다.)

새로운 행동을 추가하기 위해서 가장 보편적인 방법은 상속을 이용하는 것입니다.
허나 이는 정적이고 클래스구조가 복잡해진다는 단점이 존재합니다.
```cpp
class Window abstract { public: virtual void View(void) { } };
class BorderWindow abstract : public Window { public: void Border(void) { } };
class ScrollWindow abstract : public Window { public: void Scroll(void) { } };
class BorderScrollWindow abstract : public Window { public: void Scroll(void) { } void Border(void) { } };

class TextWindow final : public Window { public: virtual void View(void) override { } };
class BorderTextWindow final : public BorderWindow { public: virtual void View(void) override { Border(); } };
class ScrollTextWindow final : public ScrollWindow { public: virtual void View(void) override { Scroll(); } };
class ScrollTextWindow final : public BorderScrollWindow { public: virtual void View(void) override { Border(); Scroll(); } };

class DocWindow final : public Window { public: virtual void View(void) override { } };
class BorderDocWindow final : public BorderWindow { public: virtual void View(void) override { Border(); } };
class ScrollDocWindow final : public ScrollWindow { public: virtual void View(void) override { Scroll(); } };
class BorderScrollDocWindow final : public BorderScrollWindow { public: virtual void View(void) override { Border(); Scroll(); } };
```

> ## 차이점

### 복합체
장식자 패턴은 복합체 패턴과 생김새가 거의 동일합니다.

기존 클래스와 장식자/복합체 클래스의 공동 인터페이스를 선언하고
장식자/복합체 클래스에서 기존 클래스를 호출할 수 있게 만드는 점이라는 것은
장식자 패턴은 한 구성요소만을 가지는 복합체 패턴으로 볼 수 있습니다.

허나 복합체 패턴은 단일 객체와 복합 객체를 같이 제어하는데 초점을 둔것이고
장식자 패턴은 기존 단일 객체에 추가적인 기능을 집어넣기 위한 목적이라는 차이가 있습니다.
