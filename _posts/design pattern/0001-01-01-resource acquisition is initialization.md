---
title: "RAII(Resource Acquisition Is Initialization)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

RAII패턴은 C++에만 존재하는 자원 관리용 디자인 패턴입니다.<br>
Resource Acquisition Is Initialization의 약자로 직역하면 자원의 획득은 초기화다. 라는 뜻을 가집니다.<br>
<br>
자원이란 사용을 마치고 시스템에게 돌려주어아 햐는 모든 것을 말합니다.<br>
자원의 예시로는 동적 할당부터, 파일, 뮤텍스, 그래픽, 폰트, 브러시등 종류가 다양합니다.
> ## 필요성

C++에서는 가비지 컬렉션같은 자원 관리 프로세스가 존재하지 않기 때문에<br>
지원을 제대로 반환하지 않는다면 메모리 누수가 발생하게 됩니다.<br>
<br>
허나 이러한 자원들을 하나하나 수작업으로 반환해주기는 또 굉장히 번거롭고 실수할 여지가 큽니다.<br>
때문에 C++에서는 자원 관리에 대한 필요성이 존재하게 됩니다.<br>
<br>
RAII패턴은 이러한 자원 관리를 수월하게 해주는 디자인 패턴입니다.<br>
stack영역에 존재하는 객체는 scope가 끝나면 무조껀 소멸자가 호출된다는 보장을 중점으로<br>
소멸시점에 자원을 관리/정리하는 동작을 하게합니다.<br>
<br>
heap영역에 존재하는 객체는 scope에 제약받지 않지만 이를 stack 영역에 존재하는 객체로 감싸<br>
stack영역의 객체처럼 사용 후 정리 할 수 있습니다.<br>
<br>
또한 표준 라이브러리에서도 RAII 패턴을 사용하고 있습니다.
> ## 구현

RAII패턴의 구현에 앞서 동적 할당을 하기 때문에<br>
heap영역의 객체를 자원 관리 해야하는 코드를 먼저 작성해 보겠습니다.
```cpp
void function(void) {
	int* num = new int;

	... //num 사용

	delete num;
}
```
동적 할당 후 중간에 자원을 사용하고 마지막에 자원을 해제하는 코드입니다.<br>
만약 이 코드에서 중간에 예외가 발생한다면 함수를 종료해야 된다고 가정하겠습니다.
```cpp
void function(void) {
	int* num = new int;
	std::vector<int> nums{ 1 };

	try {
		*num = nums.at(1); //1번 예외(벡터의 1번째 원소의 값을 넣는다, 에러: 벡터는 0번째 원소 까지밖에 없다.)
	}
	catch (const std::exception&) {
		delete num; //delete추가
		return;
	}

	if (num != 0) { //2번 예외(첫 번째 nums.at(0)으로 수정, 에러: num은 0이 아닌 1이다.)
		delete num; //delete추가
		return;
	}

	delete num;
}
```
말이 안되는 코드이긴 하지만 강제로 에러를 낸 코드입니다.<br>
- vector의 인덱스가 0번째 까지 밖에 존재하지 않는데 1번 인덱스를 참조하려 해서 에러를 발생시키고 리턴합니다.<br>
- 또 아래쪽에서 0번 벡터의 값을 집어넣더라도 num값이 0이 아니기 때문에 리턴될 것입니다.<br>

이러한 상황에서 delete를 호출해줘야합니다.<br>
<br>
만약 동적할당된 자원이 num한개가 아니라 여러개라면 delete키워드가 엄청나게 들어가는 코드가 돼 버릴 것입니다.<br>
또는 내가 예측하지 못한 다른 곳에서 return 이 떨어지면 delete를 하지도 못한체 메모리 누수가 나게 됩니다.<br>
이러한 상황을 코드들에 전부 신경쓴다는 것은 녹록지 않은 작업입니다.<br>
<br>
다음은 RAII를 적용한 패턴입니다.
```cpp
class RAII final { //RAII 클래스
public:
	RAII(int* ptr) :
		_ptr(ptr) {
	}
	~RAII(void) {
		delete _ptr;
	}
	int& operator*() {
		return *_ptr;
	}
private:
	int* _ptr;
};

void function(void) {
	RAII num = new int; //RAII 객채
	std::vector<int> nums{ 1 };

	try {
		*num = nums.at(1);
	}
	catch (const std::exception&) {
		return; //리턴할 때 RAII 소멸자 호출
	}

	if (*num != 0) {
		return; //리턴할 때 RAII 소멸자 호출
	}
}
```
함수에서 모든 delete코드가 사라졌습니다. new int인 heap변수는 RAII클래스의 stack변수에 의해 관리되기 시작합니다.<br>
어떠한 방식으로든 함수가 리턴되면(scope 밖으로 벗어나면) stack 객체인 RAII 객체는 자동으로 소멸자를 호출합니다.<br>
<br>
이러한 방식으로 사용자가 예측하지 못한 예외나 delete를 까먹는 일이 발생하더라도<br>
RAII패턴을 사용하면 미리 문제를 예방해 줍니다.
> ## 라이브러리

앞선 코드는 int형에 대한 RAII클래스였지만<br>
C++에는 이미 이러한 RAII패턴을 사용하게 만들어주는 템플릿 라이브러리가 존재합니다.
### 스마트 포인터(중 auto_ptr)
스마트 포인터라 불리는 방식입니다.<br>
원래 auto_ptr은 문제가 생겨 현재는 사용하지 않는 방식입니다.
```cpp
std::auto_ptr<int> ptr(new int);
```
스마트 포인터는 종류가 다양하고 사용법이 따로 존재합니다.<br>
이는 스마트 포인터 글에서 작성하겠습니다.
### lock_guard
mutex또한 동적할당의 new, delete와 마찬가지로<br>
lock을 해줬으면 꼭 unlock을 해줘야한다는 규약이 있습니다.<br>
<br>
이를 RAII 패턴을 사용하여 라이브러리로 지원해주었는데 이것이 lock_guard입니다.<br>
사용법은 다음과 같습니다.
```cpp
std::mutex mutex;
std::lock_guard<std::mutex> lock(mutex);
```
lock 객체는 생성자에서는 매개 변수로 받은 mutex의 lock()함수를 호출해주고<br>
소멸자에서는 unlock()함수를 호출해 줍니다.
