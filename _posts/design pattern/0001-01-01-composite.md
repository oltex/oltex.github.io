---
title: "복합체(Composite)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

구조 디자인 패턴중 하나인 복합체 패턴입니다.<br>
<br>
부분과 전체의 계층을 표현하기 위해 객체들을 모아 트리 구조로 구성합니다.<br>
사용자로 하여금 개별 객체와 복합 객체를 모두 동일하게 다룰 수 있도록 하는 패턴입니다.
> ## 필요성

그래픽 작업을 위해 도형(Shape) 인터페이스를 만들고<br>
삼각형, 사각형, 원등의 구체 클래스를 제작했다고 합니다.<br>
도형 인터페이스에는 색깔을 채워넣기 위한 Color라는 함수가 존재합니다.<br>
<br>
도형들을 그룹으로 관리하기 위해서<br>
Group이라는 클래스를 제작하여 도형들을 담을 수 있게 만들었습니다.<br>
<br>
이제 Color함수를 통해서 내가 선택한 객체의 색을 바꿀수 있게 만들어야 합니다.<br>
그리고 그룹안에 존재하는 모든 도형들은 색이 변경되어야 합니다.<br>
<br>
문제는 내가 선택한 객체가 도형(Shape)인지 그룹(Group)인지에 대한 판단을 해줘야 합니다.
```cpp
class Shape abstract { //도형
public:
	void Color(void) {
	}
};
class Triangle final : public Shape {
};
class Rectangle final : public Shape {
};
class Sphere final : public Shape {
};

class Group final { //그룹
public:
	void Add(Shape* shape) {
		_shape.emplace_back(shape);
	}
	void Color(void) {
		for (auto iter : _shape) {
			iter->Color();
		}
	}
private:
	std::vector<Shape*> _shape;
};

void main(void) {
	Shape* shape = new Triangle; //단일 객체: 삼각형
	Group* group = new Group; //그룹 객체: 사각형, 원
	group->Add(new Rectangle);
	group->Add(new Sphere);

	shape->Color(); //결국엔 Color를 일일히 호출해줬다.(Shape인지 Group인지 판단해야한다.)
	group->Color();
}
```
마지막 main코드에서 shape랑 group의 Color함수를 각각 호출해줬습니다.<br>
<br>
같은 도형을 관리하는 클래스임에도 불구하고 단일이냐 복합 객체냐에 따라서<br>
코드가 달라지게 되는 결과를 초래합니다.<br>
이는 사용자에게 여러 분기를 처리해야하는 어려움을 줍니다.<br>
<br>
복합체는 Shape와 Group의 공통 인터페이스를 선언해 이를 해결합니다.<br>
객체와 컨테이너를 모두 표현할 수 있는 하나의 추상 클래스를 정의하는 것입니다.
> ## 구현

이번 예제에서는 Group또한 Shape를 의미하는 것이므로<br>
<br>
Shape와 Group의 상위 추상 클래스를 따로 만들지 않고<br>
Shape를 상위 추상 클래스로 하고 Group를 서브 클래스로 집어넣겠습니다.
```cpp
class Shape abstract {
public:
	virtual void Add(Shape* shape) {
	}
	virtual void Color(void) {
	}
};
class Triangle final : public Shape {
};
class Rectangle final : public Shape {
};
class Sphere final : public Shape {
};

class Group final : public Shape {
public:
	virtual void Add(Shape* shape) override {
		_shape.emplace_back(shape);
	}
	virtual void Color(void) override {
		for (auto iter : _shape) {
			iter->Color();
		}
	}
private:
	std::vector<Shape*> _shape;
};

void main(void) {
	std::vector<Shape*> shape;
	shape.emplace_back(new Triangle);
	shape.emplace_back(new Group);
	shape.at(1)->Add(new Rectangle);
	shape.at(1)->Add(new Sphere);

	for (auto iter : shape) { //이제 Color를 호출하는 대상이 Group인지 알 필요가 없다!
		iter->Color();
	}
}
```
main함수에서 이제 벡터를 사용하여 모든 Shape에 대해 관리하기 시작했습니다.<br>
<br>
이 패턴의 가장 큰 이점은 복합체를 구성하는 구체 클래스에 대해 신경 쓸 필요도 없고<br>
또 객체가 단일 객체인지, 아니면 그룹인지 알 필요가 없다는 것입니다.
