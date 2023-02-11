---
title: "장식자(Decorator)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

구조 디자인 패턴 중 하나인 장식자 패턴입니다.<br>
객체에 동적으로 새로운 행동을 추가할 수 있게 만듭니다.<br>
<br>
새로운 행동들을 포함한 장식자 객체를 추가하고 기존 객체와 연결하여 해당 행동을<br>
연결시키는 패턴입니다.<br>
> ## 필요성

장식자 패턴은 다음과 같은 상황에서 사용하면 좋습니다.
- 동적으로 각각의 객체에 기능을 추가해야합니다.
- 또한 동적으로 제거될 수 있는 기능입니다.
- 기존 코드를 훼손하고 싶지 않습니다.

화면에 창을 출력하기 위해 Window 클래스를 제작하였습니다.<br>
윈도우 클래스에는 화면을 출력하기 위한 View함수가 존재합니다.<br>
<br>
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
이후 특정 윈도우 클래스 객체는<br>
두꺼운 테두리를 추가하거나 스크롤을 추가해줄 필요가 존재해집니다.<br>
(두 가지 기능을 전부 가지고 있는 객체도 존재합니다.)<br>
<br>
새로운 행동을 추가하기 위해서 가장 보편적인 방법은 상속을 이용하는 것입니다.<br>
허나 이는 정적이고 클래스 구조가 복잡해진다는 단점이 존재합니다.<br>
<br>
코드가 쓸대없이 너무 길어져서 한줄로 작성했습니다.<br>
클래스 개수에만 주목해주세요.
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
클래스의 양이 엄청 증가되었습니다.<br>
<br>
이를 보완하기 위해<br>
객체 합성을 이용하는 방법도 존재합니다.<br>
이는 클래스 개수를 줄일순 있지만 클래스에 코드가 결합된다는 단점이 존재합니다.<br>
<br>
장식자 패턴을 사용하면 클래스량도 줄일 수 있고 컴플링을 최소화 할 수 있습니다. 
> ## 구현

위 예제를 장식자 패턴을 사용하여 구현해 보겠습니다.<br>
(코드를 간략하게 만들기위해 소멸자, delete는 적지 않았습니다.)
```cpp
class Window abstract {
public:
	virtual void View(void) {
	}
};
class BorderDecorator final : public Window { //Border 장식자
public:
	BorderDecorator(Window* window)
		: _window(window) {
	}
	virtual void View(void) override {
		//Border 처리 진행
		_window->View(); //연결된 윈도우의 View함수를 호출합니다.
	}
private:
	Window* _window = nullptr; //연결할 윈도우
};
class ScrollDecorator final : public Window { //Scroll 장식자
public:
	ScrollDecorator(Window* window)
		: _window(window) {
	}
	virtual void View(void) override {
		//Scroll 처리 진행
		_window->View();  //연결된 윈도우의 View함수를 호출합니다.
	}
private:
	Window* _window = nullptr; //연결할 윈도우
};

class TextWindow final : public Window {
public:
	virtual void View(void) override {
	}
};
class DocWindow final : public Window {
public:
	virtual void View(void) override {
	}
};

void main(void) {
	Window* text = new BorderDecorator(new TextWindow); //텍스트 윈도우에 보더 장식자를 연결합니다.
	Window* doc = new BorderDecorator(new ScrollDecorator(new DocWindow)); //문서 윈도우에 보더, 스크롤 장식자를 연결합니다.

	text->View(); //보더 장식자의 View호출->텍스트 윈도우의 View호출
	doc->View(); //보더 장식자의 View호출->스크롤 장식자의 View호출->문서 윈도우의 View호출
}
```
장식자로 연결되어 이제 클래스의 양도 증가되지 않았습니다.<br>
또 Text클래스와 Border클래스는 컴플링도 최소화 되어 있습니다.<br>
> ## 주의

### 복합체(Conposite)
장식자 패턴은 복합체 패턴과 생김새가 거의 동일합니다.<br>
<br>
기존 클래스와 장식자/복합체 클래스의 공동 인터페이스를 선언하고<br>
장식자/복합체 클래스에서 기존 클래스를 호출할 수 있게 만드는 점이라는 것은<br>
장식자 패턴은 한 구성 요소만을 가지는 복합체 패턴으로 볼 수 있습니다.<br>
<br>
허나 복합체 패턴은 단일 객체와 복합 객체를 같이 제어하는데 초점을 둔것이고<br>
장식자 패턴은 기존 단일 객체에 추가적인 기능을 집어넣기 위한 목적이라는 차이가 있습니다.
### 가교(Bridge)
필요성 파트를 보면 가교 패턴과 비슷하게<br>
상속 클래스를 두 가지 차원에서 확장해 나가려해서 생기는 문제같이 보입니다.
- 가교: Device를 Device객체(TV, Moniter)들과 API(삼성,LG) 두가지 차원에서 확장 시도
- 장식자: Window를 Window객체(Text, Doc)들과 부가 기능(Border, Scroll) 두가지 차원에서 확장 시도

필요 상황이 비슷하기 때문에 차이점을 짚고 넘어가겠습니다.
1. 가교는 객체가 가교를 사용하고 있음을 인식하지만, 장식자는 객체가 장식자를 사용하고 있음을 인식하지 못합니다.
2. 가교의 목적은 객체의 기능을 분할/선택하는데 있습니다. 장식자의 목적은 객체에 기능을 추가하는데 있습니다.

쉽게 말해 가교는 삼성TV, 엘지TV로 분할하고 싶은거고<br>
장식자는 Text를 필요가 있으면 보더Text로 기능 추가를 하고 싶은겁니다.

