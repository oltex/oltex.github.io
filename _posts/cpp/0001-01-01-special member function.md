---
title: "특수 멤버 함수(Special Member Function)"
categories:
  - cpp
tags:
  - tag
---
> ## 개요

C++에서 클래스를 선언하면 컴파일러가 암시적으로 생성하는 함수들이 존재합니다.
- 생성자
- 소멸자
- 복사 생성자
- 복사 대입 연산자
- 이동 생성자
- 이동 대입 연산자

보통 언급할 때 기본 혹은 default를 붙여서 이야기합니다.
예를 들어 기본 생성자, default 소멸자 등으로 언급할 수 있습니다.

이 특수 멤버 함수들은 사용자가 명시적으로 선언해 줄 시 암시적으로 생성되지 않습니다.
또한 특정 조건에 따라서도 암시적으로 생성되지 않을 수 있습니다.
> ## 구현

특수 멤버 함수들은 default 키워드를 사용하여 명시적으로 표현할 수 있습니다.
```cpp
class Class final {
public:
	Class(void) = default; //생성자
	~Class(void) = default; //소멸자

	Class(const Class& rhs) = default; //복사 생성자
	Class& operator=(const Class& rhs) = default; //복사 대입 연산자

	Class(Class&& rhs) noexcept = default; //이동 생성자
	Class& operator=(Class&& rhs) noexcept = default; //이동 대입 연산자
};
```
이동 생성자와 이동 대입 연산자는 noexcept로 선언해서 예외를 던지지 않음을 명시해줘야합니다.
> ## 조건

각 특수 멤버 함수는 암시적으로 생성되기도 하지만
조건에 따라서는 암시적으로 삭제될 수 있습니다.

이 조건들이 특수 맴버 함수들마다 다릅니다. 조건들은 다음과 같습니다.
### 생성자
- 생성자가 명시적 선언 되었거나 delete거나 매개 변수가 있으면 제거됩니다.
- 부모 클래스, 맴버 변수의 생성자가 delete 거나 private 거나 매개 변수가 있으면 제거됩니다.
- 부모 클래스, 맴버 변수의 소멸자가 delete 거나 private 이면 제거됩니다.
- 초기화되지 않은 const, 레퍼런스 맴버 변수가 존재하면 제거됩니다.(부모 클래스, 맴버 변수 클래스 포함)
### 소멸자
- 소멸자가 명시적 선언 되었거나 delete면 제거됩니다.
- 부모 클래스, 클래스 맴버 변수의 소멸자가 delete 거나 private면 제거됩니다.
### 복사 생성자
- 명시적으로 선언된 이동 생성자나 이동 대입 연산자가 존재하면 제거됩니다.
- 부모 클래스, 맴버 변수의 소멸자가 delete 거나 private 이면 제거됩니다.
### 복사 대입 연산자
-
### 이동 생성자
### 이동 대입 연산자
