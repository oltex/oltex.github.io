---
title: "해석자(Interpreter)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

행동 디자인 패턴중 하나인 해석자 패턴입니다.<br>
간이 언어를 만들고 이를 해석하기 위한 해석자를 생성하여 그 문장을 해석하게 만듭니다.<br>
<br>
특정한 문제가 번번히 발생할 때, 매번 알고리즘을 작성하여<br>
문제를 해결하기 보다 공통된 약속을 만들고 이를 해석하는 방법이 낫습니다.<br>
<br>
한마디로 문법 규칙을 클래스화 시켜놓은 패턴으로, 일련의 규칙을 통해 언어를 해석합니다.
> ## 예시

예를 들어 "2 add 3" 이라는 문장을 결과로 5를 도출하고 싶다면<br>
그에 맞는 해석자를 생성하여 해당 구문을 해석하게 만들면 됩니다.<br>

가장 대표적인 예시로
어떤 패턴에 일치되는 문자열을 찾는 문제는 보통 정규표현식을 사용하여 구현합니다.<br>
이 때 정규표현식을 해석하는 작업이 해석자 패턴을 통해 이뤄집니다.
> ## 구조

해석자 패턴은 디자인 패턴중 어려운 측에 속하기 때문에<br>
구조부터 알아보겠습니다.<br>
<br>
구조를 쉽게 파악하기 위해<br>
number를 +, - 연산하는 해석자를 예로 들어보겠습니다.
### Expression
기호라는 뜻을 가진 추상 클래스입니다.
추상 구문 트리를 생성하며 모든 노드에서 가져야 할 Interpret()연산을 정의합니다.
### Terminal
단말 기호라는 뜻으로
더 이상 해석할 수 없는 고정된 값을 가지는 기호를 의미합니다.
위 예시로 number에 해당합니다.
### Nonterminal
비단말 기호를 의미합니다.
해석 연산을 할 수 있는 기호로 연산을 통해 다른 비단말 기호나 단말 기호를 도출해냅니다.
위 예시로 +, - 연산에 해당합니다.
### Context
포괄적인 정보를 포함합니다.
해석자에 들어가는 문장을 가지고 있습니다.
> ## 구현

후위 표기법 방식으로 숫자를 덧셈하는 연산을 해석자를 구현해보겠습니다.<br>
(가볍게 만들기 위해 소멸자나 delete는 적지 않았습니다.)
```cpp
class Expression abstract {
public:
	virtual int Interpret(void) = 0;
};
```
```cpp
class NumberTerminal final : public Expression {
public:
	NumberTerminal(std::string str) :
		_number(std::stoi(str)) {
	}
	virtual int Interpret(void) override {
		return _number;
	}
private:
	int _number = 0;
};
```
```cpp
class AddNonterminal final : public Expression {
public:
	AddNonterminal(Expression* exp0, Expression* exp1) :
		_exp0(exp0),
		_exp1(exp1) {
	}
	virtual int Interpret(void) override {
		return _exp0->Interpret() + _exp1->Interpret();
	}
private:
	Expression* _exp0 = nullptr;
	Expression* _exp1 = nullptr;
};
```
```cpp
class Context final {
public:
	Context(std::string str) {
		_str = str;
	}
	std::string operator[](int _index) {
		return std::string{ _str[_index] };
	}
private:
	int _index = 0;
	std::string _str;
};
```
```cpp
void main(void) {
	Expression* expression = nullptr;
	Context context{ "11+4+" };
	
	//Context를 해석하여 각 터미널과 논터미널로 치환합니다.
	//여기서는 수동으로 치환했습니다.
	NumberTerminal* number0 = new NumberTerminal{ context[0] };
	NumberTerminal* number1 = new NumberTerminal{ context[1] };
	AddNonterminal* add2  = new AddNonterminal{ number0, number1 };
	NumberTerminal* number3 = new NumberTerminal{ context[3] };
	AddNonterminal* add4 = new AddNonterminal{ add2, number3 };

	expression = add4;

	std::cout << expression->Interpret() << std::endl;
};
```
