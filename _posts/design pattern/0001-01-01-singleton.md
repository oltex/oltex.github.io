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
<br>
예를들어 응용 프로그램은 여러 클래스들로 이루어져 있지만 시스템과 통신하여 그래픽을 출력하는 클래스는<br>
응용프로그램 내에 한개만 존재하면 충분할 것입니다.<br>

두 번째로 디버그를 클래스가 있다고 하면 어느곳이든 다 접근이 가능해야 할 것입니다.<br>
이러한 경우 싱글턴을 사용하기에 적합합니다.<br>
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
			_instance = new Graphic;
		return _instance;
	}
	static void Destroy(void) {
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
전역적인 instance를 가지게 되었습니다.<br>
두 번째 호출부터는 같은 instance를 호출하게 되어 인스턴스의 개수가 한개임을 보장합니다.<br>
마지막으로 프로그램 종료시 Destroy()로 메모리 할당 해제 작업을 추가로 해줘야합니다.<br>
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
또한 추가적으로 메모리 할당 해제 작업이 필요없습니다.<br>
<br>
단점은 프로그램이 끝날때까지 싱글톤 객체를 지울수 없다는 점과<br>
포인터의 이점을 사용하지 못한다는 점이 있습니다.
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
<br>
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
	friend Graphic* Singleton<Graphic>::Instance(); //추가 설명
	friend void Singleton<Graphic>::Destory(); //추가 설명
private:
	Graphic(void) = default;
	virtual ~Graphic(void) override = default;
};
```
Instance() 함수에서 new Graphic를 하기 위해서 Graphic의 생성자에 접근할 필요가 존재합니다.<br>
하지만 Graphic의 생성자를 public으로 두면 외부에서 Graphic 객체가 생성 가능하기 때문에 friend키워드를 사용하여 한정적인 접근을 허용합니다.<br>
같은 의미로 Destory() 함수에서 delete Graphic을 하기 위해 Graphic의 소멸자에 접근해야 되기 때문에 friend키워드를 사용하였습니다.<br>
<br>
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
	friend Singleton<Grpahic>; //추가 설명
private:
	Graphic(void) = default;
	virtual ~Graphic(void) override = default;
};
```
맴버 변수 버전과 다르게 friend 키워드의 접근범위를 Singleton\<Grpahic\>클래스 전체로 바꾸었습니다.<br>
여기서부터는 추측입니다.(정보를 찾지 못했습니다.ㅠㅠ)<br>
<br>
Singelton\<Graphic\>::Instance()에서 생성되는 지역 변수인 static Graphic \_instance가 프로그램 종료시 소멸을 위해 Graphic 소멸자를 호출하는데<br>
이것이 Singelton\<Graphic>::Instance()함수가 아닌 다른 곳에서 이루어지는 것 같습니다.<br>
따라서 friend 키워드를 Singleton\<Graphic>::Instance()로 한정지으면 ~Graphic()에 접근하지 못해서 에러를 발생시킵니다.<br>
그렇기에 해결책으로 Singleton\<Graphic\> 전역에 friend 키워드를 선언해 주었습니다.
### 상속/다형성
같은 상속을 이용하는 코드이지만 이번에는 생성이 아닌 다형성에 초점을 둔 코드입니다.<br>
사실 다형성이라 부르기 애매합니다.<br>
인터페이스를 통해 여러가지 객체를 만드는 것이 아니라 그중 하나의 객체만을 만드는 방법입니다.<br>
허나 다형성 방식의 코딩을 응용하고 있으니 다형성이라 칭하겠습니다.<br>
<br>
예를 들어 싱글톤으로 존재해야하는 FileSystem이라는 클래스가 존재한다고 가정해봅시다.<br>
이 FileSystem을 PlayStation과 Nintendo 두 멀티플랫폼에서 지원해야 한다고 하면 어떻게 할 수 있을까요?<br>
<br>
다음과 같은 코드를 사용해보겠습니다.<br>
(static 맴버 변수 방식만 가능합니다.)
```cpp
class FileSystem {
public:
	static FileSystem* const& Instance(void); //구현부는 아래쪽에
	virtual void Function(void) = 0;
private:
	static FileSystem* _instance;
};
FileSystem* FileSystem::_instance = nullptr;

class PlayStationFileSystem final : public FileSystem {
public:
	virtual void Function(void) override { };
};
class NintendoFileSystem final : public FileSystem {
public:
	virtual void Function(void) override { };
};

FileSystem* const& FileSystem::Instance(void) { //구현부입니다.
	if (nullptr == _instance)
#ifdef PLAYSTATION
		_instance = new PlayStationFileSystem;
#elif NINTENDO
		_instance = new NintendoFileSystem;
#endif
	return _instance;
}
```
싱글톤 클래스를 멀티 플랫폼에 대응할 수 있게 만들었습니다.
> ## 문제점

싱글톤은 사실 굉장히 신중히 사용해야하거나 애초에 사용을 지양해야 한다고 합니다.
싱글톤을 사용함으로 인해 생기는 이점보다 단점이 더 많기때문이라고 합니다.
### 전역변수
싱글톤의 강력한 효과중 하나가 전역 변수와 다를게 없다라는 것입니다.<br>
때문에 전역 변수를 사용하게되면 생기는 문제를 같이 얹고 가게됩니다.
### 의존성
싱글톤은 인터페이스보다 구현 클래스에 더 집중하게되는 코드입니다.<br>
인터페이스를 통하지 않으면 싱글톤을 사용하는 곳과 싱글톤 클래스간의 의존적인 관계가 생긴다는것을 의미합니다. (SOLID의 DIP)<br>
또한 여러 클래스가 접근 가능하다는 점에서 클래스간의 결합도가 높아집니다.<br>
또한 싱글턴을 사용하면 아무나 수정하고 공유되는 전역 상태가 존재한다는 것은 객채지향 프로그래밍에 맞지 않습니다.(SOLID의 OCP)
### 테스트
싱글톤은 테스트 또한 어렵게 만듭니다.<br>
싱글톤의 생성은 굉장히 제한적이기 때문에 테스트를 위한 Mock객체를 만들기 어렵습니다.<br>
테스트 코드를 작성하기 어렵다는 점은 개발에 큰 걸림돌이 됩니다.
### 멀티 스레드
싱글톤을 사용하면 멀티 스레드환경에서 전역변수가 생긴다는 것을 의미합니다.<br>
이는 동기화 문제나 객체가 두개 생성되는 문제를 야기할 수 있습니다.
> ## 멀티 스레드

### 문제
싱글턴의 멀티 스레드 첫 번째 문제점은 두가지 객체가 만들어질 수 있는 상황이 존재한다는 것입니다.
다시 한번 코드를 보겠습니다. 싱글톤의 생성 부분입니다.
```cpp
class Singleton final {
public:
	static Singleton* const& Instance(void) {
		if (nullptr == _instance) //두 개의 스레드가 동시접근!
			_instance = new Singleton; //두 개의 스레드가 동시생성! 
		return _instance;
	}
private:
	static Singleton* _instance;
};
Singleton* Singleton::_instance = nullptr;
```
만약 두 개의 스레드에서 동시에 Instance()함수를 호출했다고 가정해봅시다.<br>
동시에 nullptr 검사를 진행한다면 두 스래드 전부 if문 안을 타고 들어가 동적 할당을 시작할 것입니다.<br>
이런 방식으로 2개의 싱글톤 객체가 생길 수 있다는 문제가 존재합니다.
### 해결
이러한 문제를 해결하기위해 사용되는 여러 방법들이 존재합니다.
#### 더블 체크 락킹(DCL)
Double Checked Locking 방식입니다.<br>
두 번 체크를 하면서 lock을 해줘서 스레드의 동시 접근을 막는 방법입니다.
```cpp
class Singleton final {
public:
	static  Singleton* const& Instance(void) {
		if (nullptr == _instance) {
			_mutex.lock(); //RAII 방식: std::lock_guard<std::mutex> lock{ _mutex }; 
			if (nullptr == _instance)
				_instance = new Singleton;
			_mutex.unlock();
		}
		return _instance;
	}
private:
	static Singleton* _instance;
	static std::mutex _mutex;
};
Singleton* Singleton::_instance = nullptr;
```
nullptr 검사를 해주고 lock을 걸어 접근을 제한한 다음에 다시 nullptr 검사를 해주는 방식을 통해서<br>
성능 하락또한 막으면서도 두 스레드의 동시 접근또한 막을 수 있습니다.<br>
<br>
이론상 완벽해보이는 코드이지만 여기에는 두 가지 문제점이 존재합니다.<br>
첫 번째는 컴파일러 최적화이고 두 번째는 CPU의 메모리 아키텍처입니다.<br>
<br>
첫 번째 문제를 보면 컴파일러는 같은 결과를 보장하는 한해서 코드를 훨씬 빠르게 재배치(reordering)하는 기능이 존재하기 때문에<br>
코드의 순서가 바뀔 수 있습니다. 문제는 이것이 멀티 스레드를 고려하지 않는다는 것이죠<br>
<br>
new 키워드도 이러한 최적화 과정이 존재합니다. new 키워드는 한줄이지만 3가지 단계로 구성되어있습니다.<br>
메모리 할당 -> 객체 초기화 -> 메모리 주소를 변수에 저장<br>
이 과정에 최적화를 진행하게 되면 메모리를 할당하는 작업과 변수에 저장하는 작업을 합쳐버립니다.<br>
메모리 할당 -> 메모리 주소를 변수에 저장 -> 객체 초기화<br>
이로 인해서 초기화 과정 전에 메모리에 주소가 들어가있게 되는데 이는 아래 문제점과 겹쳐 문제를 일으킵니다.<br>
<br>
두 번째 문제는 CPU의 메모리 아키텍처인데 CPU에 캐시 메모리가 있다는 것을 생각해봅시다.
- 스레드1에서 lock을 걸고 메모리 할당을 성공하고 초기화도 시도하려 합니다.
- 허나 할당된 주소값은 캐시 메모리를 거쳐 메모리에 들어가버렸지만
- 초기화는 과정이 길고 복잡해 바로 메모리로 가지않고 일부가 캐시 메모리에 머무를 수 있습니다.
- 그러던 중에 초기화가 끝나면 unlock이 실행될 수 있는데
- 일부가 캐시 메모리에 남아있는 중 스레드2에서 다시 lock을 걸고 완벽히 초기화되지 않은 공간에 접근해 버리게 됩니다.(캐시 일관성 문제)

---
이러한 과정은 전부 날려버리는 키워드가 존재합니다. 바로 volatile이라는 키워드입니다.<br>
<br>
volatile 키워드는 변수가 외부 요인에 의해 언제든지 변경될 수 있다라는 것을 알려주는 키워드입니다.<br>
쉽게 말해 언제든 바뀔 수 있으니 허튼짓(최적화) 하지 말고 내가 짜둔 코드대로 진행하라는 것이죠.<br>
게다가 이 키워드는 언제든 변경될 수 있으니 캐시로 만들지도 말라고 합니다.<br>
위에서 나온 2가지 문제를 동시에 처리하는 키워드 입니다만 이것또한 문제가 존재합니다.<br>
<br>
먼저 코드를 보겠습니다.<br>
```cpp
class Singleton final {
public:
	static volatile Singleton* volatile const& Instance(void) {
		if (nullptr == _instance) {
			_mutex.lock();
			if (nullptr == _instance)
				_instance = new volatile Singleton;
			_mutex.unlock();
		}
		return _instance;
	}
private:
	static volatile Singleton* volatile _instance;
	static std::mutex _mutex;
};
volatile Singleton* volatile Singleton::_instance = nullptr;
```
코드에 volatile 키워드가 엄청 많이 추가되었습니다.<br>
이렇게 하면 캐시 메모리를 사용하지 않고 자체적인 최적화도 막기 때문에 문제가 없어보이지만 또다른 문제가 존재합니다.<br>
<br>
먼저 싱글톤 클래스에 다른 변수인 int x, y, z가 존재한다라고 하면 이들도 캐시 메모리를 사용할 수 있기 때문에<br>
모든 변수에 volatile 키워드를 붙여줘야합니다.<br>
추가로 함수도 마찬가지로 모든 함수 또한 volatile 키워드를 붙여줘야함은 덤이지요.<br>
<br>
단지 생성에서 문제를 막고싶었을 뿐인데 확장성이 엄청 떨어지는데다가<br>
최적화를 전부 막아버린 클래스라니 듣기 만해도 성능 떨어지는 소리가 들립니다.<br>
<br>
이러한 이유에서 DCL 방식은 현재 사용을 권장하지 않는 방식이라고 합니다.
### 이른 초기화
이른 초기화 방식은 

### LazyHolder


