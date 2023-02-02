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
먼저 싱글톤을 static 맴버 변수로 선언하여 사용하는 방법입니다.
```cpp
class Graphic final {
public:
	static Graphic* const& Instance(void) {
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
	Graphic* const& graphic = Graphic::Instance();
}
```
static 맴버 변수를 선언하고 이것을 static 맴버 함수인 Instance() 호출 시점에서 생성 함으로써<br>
전역적인 Graphic instance를 가지게 되었습니다.<br>
두 번째 호출부터는 같은 instance를 호출하게 되어 인스턴스의 개수가 한개임을 보장합니다.<br>
마지막으로 프로그램 종료시 instance의 메모리 할당 해제 작업을 추가로 해줘야합니다.<br>
<br>
instance를 포인터로 선언하는 이유는 번역 단위 초기화의 위험성에서 벗어나기 위해 게으른 초기화를 사용함과<br>
Graphic클래스를 한번도 호출하지 않는다면 생성이 되지않아 메모리 낭비를 막을수 있다는 장점이 있기 때문입니다.<br>
<br>
다른 방법으로 static 지역 변수로 집어넣는 방법이 존재합니다.
```cpp
class Graphic final {
public:
	static Graphic& Instance(void) {
		static Graphic _instance;
		return _instance;
	}
};
```
이 방식 또한 함수가 실행되기 이전에는 지역 변수가 생성되지 않기 때문에 메모리 낭비 문제에서 벗어납니다.<br>
또한 추가적으로 메모리 할당 해제 작업이 필요없으며,<br>
멀티 스레드 사용 시의 문제에서도 벗어날 수 있다합니다.(C++11 이상)<br>
멀티 스레드 문제점은 나중에 서술하겠습니다.<br>
<br>
단점은 프로그램이 끝날때까지 싱글톤 객체를 지울수 없다는 점과<br>
다형성을 사용하지 못한다는 점이 있습니다.
### 생성 방지
사실 위쪽 선언만으로 싱글톤 패턴을 완성했다고 보기 어렵습니다.<br>
싱글톤 패턴의 특징중 하나인 인스턴스가 오직 하나여야함을 보장하지 않았기 때문입니다.<br>
<br>
이를 보장하기 위해서는 기본 생성자, 기본 복사/이동 생성자, 기본 복사/이동 대입 연산자를 명시적으로 선언해<br>
의도치 않은 생성 및 복사/이동을 막아야 합니다.<br>
추가로 싱글톤은 소멸자도 private로 선언해주는것을 권장합니다.
```cpp
class Graphic final {
private:
	explicit Graphic(void) = default; //생성자
	explicit Graphic(const Graphic& rhs) = delete; //복사 생성자
	Graphic& operator=(const Graphic& rhs) = delete; //복사 대입 연산자

	explicit Graphic(const Graphic&& rhs) noexcept = delete; //이동 생성자 (굳이 필요 없음)
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
### 매크로
사실 싱글톤은 그 생김새가 어떤 클래스라도 똑같은 경우가 많습니다.<br>
이러한 이유로 매크로를 사용하여 싱글톤을 구현하는 방법이 존재합니다.<br>
static 맴버 변수 버전
```cpp
#define SINGLETON_DECLARE(Singleton) \
private: \
	explicit Singleton(const Singleton& rhs) = delete; \
	Singleton& operator=(const Singleton& rhs) = delete; \
public: \
	static Singleton* const& Instance(void); \
	static void Destroy(void); \
private: \
	static Singleton* _instance;
#define SINGLETON_IMPLEMENT(Singleton) \
Singleton* Singleton::_instance = nullptr; \
Singleton* const& Singleton::Instance(void) { \
	if (nullptr == _instance) \
		_instance = new Singleton; \
	return _instance; \
} \
void Singleton::Destroy(void) {	\
	if (nullptr == Instance) \
		return; \
	delete _instance; \
	_instance = nullptr; \
}
class Graphic final {
	SINGLETON_DECLARE(Graphic)
private:
	explicit Graphic(void) = default;
	virtual ~Graphic(void) = default;
};
SINGLETON_IMPLEMENT(Graphic);
```
static 지역 변수 버전
```cpp
#define SINGLETON(Singleton) \
private: \
	explicit Singleton(const Singleton& rhs) = delete; \
	Singleton& operator=(const Singleton& rhs) = delete; \
public: \
	static Singleton& Instance(void) { \
		static Singleton _instance; \
		return _instance; \
	}
class Graphic final {
	SINGLETON(Graphic)
private:
	explicit Graphic(void) = default;
	virtual ~Graphic(void) = default;
};
```
사실 기존 코드를 단순히 매크로로 빼놓는 작업에 불과합니다.<br>
매크로를 사용하여 싱글톤을 사용하면 Graphic클래스에 들어가는 코드의 양을 줄일 수 있습니다.<br>
만약 매크로 사용을 하고싶지 않다면 아래 방법을 사용하는것도 생각해볼만 합니다.
### 상속/템플릿
싱글톤 클래스를 만들고 상속과 템플릿을 이용하여 구현하는 방법입니다.<br>
이 방법으로 구현시 조금 변경 사항이 있으니 주석을 잘 확인해야합니다.<br>
static 맴버 변수 버전
```cpp
template<typename T>
class Singleton {
protected: //상속을 하기위해 private가 아닌 protected입니다.
	explicit Singleton(void) = default;
	virtual ~Singleton(void) = default;
private:
	explicit Singleton(const Singleton& rhs) = delete;
	Singleton& operator=(const Singleton& rhs) = delete;
public:
	static T* Instance(void) {
		if (nullptr == _instance)
			_instance = new T;
		return _instance;
	}
	static void Destory(void) {
		if (nullptr == _instance)
			return;
		delete _instance;
		_instance = nullptr;
	}
private:
	static T* _instance;
};
template <typename T>
T* Singleton<T>::_instance = nullptr;
class Graphic final : public Singleton<Graphic> {
	friend Graphic* Singleton<Graphic>::Instance(); //추가 설명: 1번
	friend void Singleton<Graphic>::Destory();
private:
	Graphic(void) = default;
	virtual ~Graphic(void) override = default;
};
```
static 지역 변수 버전
```cpp
template<typename T>
class Singleton {
protected:
	explicit Singleton(void) = default;
	virtual ~Singleton(void) = default;
private:
	explicit Singleton(const Singleton& rhs) = delete;
	Singleton& operator=(const Singleton& rhs) = delete;
public:
	static T& Instance(void) {
		static T _instance;
		return _instance;
	}
};
class Graphic final : public Singleton<Graphic> {
	friend Singleton<Grpahic>; //추가 설명: 2번
private:
	Graphic(void) = default;
	virtual ~Graphic(void) override = default;
};
```
추가 설명<br>
1번)<br>
Singleton<Graphic>::Instance() 함수에서 new Graphic를 하기 위해서 Graphic의 생성자에 접근할 필요가 존재합니다.<br>
하지만 Graphic의 생성자를 public으로 두면 외부에서 Graphic 객체가 생성 가능하기 때문에 friend키워드를 사용하여 한정적인 접근을 허용합니다.<br>
같은 의미로 Singleton<Graphic>::Destory() 함수에서 delete Graphic을 하기 위해 Graphic의 소멸자에 접근해야 되기 때문에 friend키워드를 사용하였습니다.<br>
2번)<br>
맴버 변수 버전과 다르게 friend 키워드의 접근범위를 Singleton클래스 전체로 바꾸었습니다.<br>
여기서부터는 추측입니다.(정보를 찾지 못했습니다.ㅠㅠ)<br>
Singelton\<Graphic>::Instance()에서 생성되는 지역 변수인 static Graphic _instance가 프로그램 종료시 소멸을 위해 Graphic 소멸자를 호출하는데<br>
이것이 Singelton\<Graphic>::Instance()함수가 아닌 다른 곳에서 이루어지는 것 같습니다.<br>
따라서 friend 키워드를 Singleton\<Graphic>::Instance()로 한정지으면 ~Graphic()에 접근하지 못해서 에러를 발생시킵니다.<br>
그렇기에 해결책으로 Singleton\<Graphic\> 전역에 friend 키워드를 선언해준 것입니다.
### 상속/다형성
같은 상속을 이용하는 코드이지만 이번에는 생성이 아닌 다형성에 초점을 둔 코드입니다.<br>
예를 들어 싱글톤으로 존재해야하는 FileSystem이라는 클래스가 존재한다고 가정해봅시다.<br>
이 FileSystem을 PlayStation과 Nintendo 두 멀티플랫폼에서 지원해야 한다고 하면 어떻게 할 수 있을까요?<br>
<br>
다음과 같은 코드를 사용해보겠습니다.<br>
(static 맴버 변수 방식만 가능합니다. 포인터/다형성)
```cpp
class FileSystem {
public:
	static FileSystem* const& Instance(void);
	virtual void Function(void) = 0;
private:
	static FileSystem* _instance;
};
FileSystem* FileSystem::_instance = nullptr;
class PlayStationFileSystem : public FileSystem {
public:
	virtual void Function(void) override {
	};
};
class NintendoFileSystem : public FileSystem {
public:
	virtual void Function(void) override {
	};
};
FileSystem* const& FileSystem::Instance(void){
	if (nullptr == _instance)
#ifdef PLAYSTATION
		_instance = new PlayStationFileSystem;
#elif NINTENDO
		_instance = new NintendoFileSystem;
#endif
	return _instance;
}
```
다형성을 사용해서 싱글톤 클래스를 멀티 플랫폼에 대응할 수 있게 만들었습니다.
> ## 문제점

싱글톤은 사실 굉장히 신중히 사용해야하거나 애초에 사용을 지양해야 한다고 합니다.

### 전역변수

### 게으른 초기화

> ## 멀티스래드
