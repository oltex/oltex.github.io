---
title: "단일체(Singleton)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

싱글톤 패턴은 디자인 생성 패턴중 하나입니다.
오직 한개의 인스턴스만을 갖도록 보장하고 이 인스턴스에 대해 전역적인 접근을 제공합니다.

> ## 필요성

어떤 클래스는 하나의 인스턴스만을 갖도록 하는 것이 좋습니다.
예를들어 응용 프로그램은 여러 클래스들로 이루어져 있지만 시스템과 통신하여 그래픽을 출력하는 클래스는
응용프로그램 내에 한개만 존재하면 충분할 것입니다.

싱글톤 패턴은 다음과 같은 상황에서 사용하면 좋습니다.
- 인스턴스가 오직 하나여야 함을 보장해야할 때
- 모든 사용자가 접근할 수 있도록 해야 할 때


> ## 구현
싱글톤의 구현은 다음과 같습니다.

### 선언
먼저 싱글톤을 선언하여 사용하는 방법이 담긴 코드입니다.
```cpp
class Graphic {
public:
	static Graphic* Instance(void) {
		if (nullptr == m_pInstance)
			m_pInstance = new Graphic;
		return m_pInstance;
	}
private:
	static Graphic* m_pInstance;
};
Graphic* Graphic::m_pInstance = nullptr;
void main(void) {
	 Graphic::Instance();
}
```
static 맴버 변수를 선언하고 이것을 static 맴버 함수인 Instance() 호출 시점에서 생성 함으로써
전역적인 Graphic instance를 가지게 되었습니다.
두 번째 호출부터는 같은 instance를 호출하게 되어 인스턴스의 개수가 한개임을 보장합니다.
물론 이후 프로그램 종료시 instance의 메모리 할당 해제작업을 추가로 해줘야합니다.

다른 방법으로 static 지역 변수로 집어넣는 방법이 존재합니다.
```cpp
class Graphic {
public:
	static Graphic* Instance(void) {
		static Graphic* _instance = new Graphic;
		return _instance;
	}
};
```

instance를 포인터로 선언하는 이유는 번역단위 초기화의 위험성에서 벗어나기 위해 늦은초기화를 사용함과
(예를 들어 Graphic클래스가 필요한 Shader 클래스가 다른 파일에 존재할 때 Graphic가 포인터가 아니라면 초기화가 되어있지 않을 수 있다는 문제) 
Graphic클래스를 한번도 호출하지 않는다면 생성이 되지않아 메모리 낭비를 막을수 있다는 장점이 있기 때문입니다.

### 생성 방지

> ##

> ## 멀티스래드

> ## 싱글톤을 사용하지 않는 이유

### 전역변수

### 게으른 초기화
