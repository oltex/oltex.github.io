---
title: "단일체(Singleton)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

싱글톤 패턴은 디자인 생성 패턴중 하나입니다.<br>
오직 한개의 인스턴스만을 갖도록 보장하고 이 인스턴스에 대해 전역적인 접근을 제공합니다.

> ## 필요성

어떤 클래스는 하나의 인스턴스만을 갖도록 하는 것이 좋습니다.<br>
예를들어 응용 프로그램은 여러 클래스들로 이루어져 있지만 시스템과 통신하여 그래픽을 출력하는 클래스는<br>
응용프로그램 내에 한개만 존재하면 충분할 것입니다.<br>
<br>
싱글톤 패턴은 다음과 같은 상황에서 사용하면 좋습니다.
- 인스턴스가 오직 하나여야 함을 보장해야할 때
- 모든 사용자가 접근할 수 있도록 해야 할 때

> ## 구현
싱글톤의 구현은 다음과 같습니다.<br>
이른 초기화 방식이나 동기화 방식등 여러가지가 있지만 일단 가장 보편적인 초기화 방식에 대해 서술하겠습니다.
### 선언
먼저 싱글톤을 맴버 변수로 선언하여 사용하는 방법입니다.
```cpp
class Graphic {
public:
	static Graphic* Instance(void) {
		if (nullptr == _instance)
			m_pInstance = new Graphic;
		return _instance;
	}
	static void Destory(void) {
		if (nullptr != _instance) {
			delete _instance;
			_instance = nullptr;
		}
	}
private:
	static Graphic* _instance;
};
Graphic* Graphic::_instance = nullptr;
void main(void) {
	Graphic* graphic = Graphic::Instance();
}
```
static 맴버 변수를 선언하고 이것을 static 맴버 함수인 Instance() 호출 시점에서 생성 함으로써<br>
전역적인 Graphic instance를 가지게 되었습니다.<br>
두 번째 호출부터는 같은 instance를 호출하게 되어 인스턴스의 개수가 한개임을 보장합니다.<br>
마지막으로 프로그램 종료시 instance의 메모리 할당 해제 작업을 추가로 해줘야합니다.<br>
<br>
instance를 포인터로 선언하는 이유는 번역단위 초기화의 위험성에서 벗어나기 위해 늦은초기화를 사용함과<br>
(예를 들어 Graphic클래스가 필요한 Shader 클래스가 다른 파일에 존재할 때 Graphic가 포인터가 아니라면 초기화가 되어있지 않을 수 있다는 문제)<br>
Graphic클래스를 한번도 호출하지 않는다면 생성이 되지않아 메모리 낭비를 막을수 있다는 장점이 있기 때문입니다.<br>
<br>
다른 방법으로 static 지역 변수로 집어넣는 방법이 존재합니다.
```cpp
class Graphic {
public:
	static Graphic& Instance(void) {
		static Graphic _instance;
		return _instance;
	}
};
```
이 방식 또한 함수가 실행되기 이전에는 지역 변수가 생성되지 않기 때문에 메모리 낭비 문제에서 벗어납니다.
또한 추가적으로 메모리 할당 해제 작업이 필요없으며,
멀티 스레드 사용 시의 문제에서도 벗어날 수 있다합니다.(C++11 이상)
멀티 스레드 문제점은 나중에 서술하겠습니다.

단점은 프로그램이 끝날때까지 싱글톤 객체를 지울수 없다는 점이있습니다.
### 생성 방지
사실 위쪽 선언만으로 싱글톤 패턴을 완성했다고 보기 어렵습니다.
싱글톤 패턴의 특징중 하나인 인스턴스가 오직 하나여야함을 보장하지 않았기 때문입니다.

이를 보장하기 위해서는 기본 생성자, 기본 복사/이동 생성자, 기본 복사/이동 대입 연산자를 명시적으로 선언해
의도치 않은 생성 및 복사/이동을 막아야 합니다.
추가로 싱글톤은 소멸자도 private로 선언해주는것을 권장합니다.
```cpp
class Graphic {
private:
	Graphic(void) = default; //생성자
	Graphic(const Graphic& rhs) = delete; //복사 생성자
	Graphic& operator=(const Graphic& rhs) = delete; //복사 대입 연산자

	Graphic(const Graphic&& rhs) noexcept = delete; //이동 생성자 (굳이 필요 없음)
	Graphic& operator=(const Graphic&& rhs) noexcept = delete; //이동 대입 연산자 (굳이 필요 없음)

	~Graphic(void) = default; //소멸자
};
void main(void) {
	Graphic graphic; //생성자: private이라 호출 불가
	Graphic graphicCopy{ graphic }; //복사 생성자: private / delete라 호출 불가
	graphicCopy = graphic; //복사 대입 연산자: private / delete리 호출 불가

	Graphic graphicMove{ std::move(graphic) }; //이동 생성자: private / delete라 호출 불가
	graphicMove = Graphic{}; //이동 대입 연산자: private / delete라 호출 불가
}
```
이동 생성자와 이동 대입 연산자는 각각 아래 조건을 만족시키면 암시적 생성이 되지 않기 때문에 위 코드에서 제외해도 됩니다.
- 이동 생성자: 명시적 복사 생성자, 복사 대입 연산자, 이동 대입 연산자, 소멸자를 선언했을 경우
- 이동 대입 연산자: 명시적 복사 생성자, 복사 대입 연산자, 이동 생성자, 소멸자를 선언했을 경우

쉽게 말해서 생성자, 복사 생성자와 복사 대입 연산자만 작성해주면 됩니다.

> ## 사용
싱글톤을 사용하는 여러가지 방법들에 대해 서술하겠습니다.
### 상속/템플릿
사실 싱글톤은 그 생김새가 어떤 클래스라도 똑같은 경우가 많습니다.
이러한 이유로 싱글톤 클래스를 만들고 상속과 템플릿을 이용하여 구현하는 방법이 존재합니다.
```cpp
template<typename T>
class Singleton {
protected:
	Singleton(void) = default;
	virtual ~Singleton(void) = default;
private:
	Singleton(const Singleton& rhs) = delete;
	Singleton& operator=(const Singleton& rhs) = delete;
public:
	static T* Instance(void) {
		if (nullptr == _instance)
			_instance = new T;
		return _instance;
	}
	static void Destory(void) {
		if (nullptr != _instance) {
			delete _instance;
			_instance = nullptr;
		}
	}
private:
	static T* _instance;
};
template <typename T>
T* Singleton<T>::_instance = nullptr;
class Graphic final : public Singleton<Graphic> {
	friend Graphic* Singleton<Graphic>::Instance();
	friend void Singleton<Graphic>::Destory();
private:
	Graphic(void) = default;
	virtual ~Graphic(void) override = default;
};
```

```cpp
template<typename T>
class Singleton {
protected:
	Singleton(void) = default;
	virtual ~Singleton(void) = default;
private:
	Singleton(const Singleton& rhs) = delete;
	Singleton& operator=(const Singleton& rhs) = delete;
public:
	static T& Instance(void) {
		static T _instance;
		return _instance;
	}
};
class Graphic final : public Singleton<Graphic> {
	friend Singleton;
private:
	Graphic(void) = default;
	virtual ~Graphic(void) override = default;
};
```

> ## 싱글톤을 사용하지 않는 이유

### 전역변수

### 게으른 초기화

> ## 멀티스래드
