---
title: "객체 풀(Object Pool)"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

오브젝트 풀(객체 풀) 디자인 패턴입니다.<br>
객체를 매번 할당/해제 하지 않고 풀에 들어가 있는 객체를 재사용함으로써 성능을 향상시킵니다.
> ## 필요성

게임을 제작하다 보면 많은 수의 오브젝트들이 생성되야 되는 경우가 존재합니다.<br>
예를 들어 보겠습니다.<br>
<br>
FPS게임을 제작하고 있다고 합니다.<br>
<br>
슈팅 게임의 기본이 되는 Gun 클래스를 만들고,<br>
총을 발사하기 위해 Shot 함수를 실행 할 수 있게 만들었습니다.<br>
<br>
Shot 함수가 실행되면 Bullet 클래스의 객체가 생성(메모리 할당)되고 이 총알은<br>
정해진 방향으로 전진하다 부딛히면 소멸(메모리 해제)하게 됩니다.<br>
<br>
많은 수의 플레이어/ 혹은 인공지능들이 게임에 참여했다고 합니다.
```cpp
class Bullet final {
public:
	bool Hit(void) { //총알이 부딛혔는지 확인합니다.
		delete this;
		return true;
	}
};
```
```cpp
class Gun final {
public:
	Bullet* Shot(void) { //총을 쏩니다.
		return new Bullet;
	}
};
```
```cpp
void main(void) {
	std::vector<Gun*> guns{ new Gun, new Gun, new Gun, new Gun, new Gun }; //총이 5개 있습니다.
	std::vector<Bullet*> bullets; //총알을 담을 컬렉션입니다.

	while (true) {
		for (auto& iter : guns)
			bullets.emplace_back(iter->Shot()); //매프레임 총을 쏩니다.

		for (auto iter = bullets.begin(); iter != bullets.end();)
			if ((*iter)->Hit()) //총알이 부딛혔을 경우
				iter = bullets.erase(iter); //총알을 지웁니다.
			else
				++iter;
	}
};
```
게임이 어느정도 진행되니 갑자기 사용자가 랙을 먹기 시작합니다.<br>
<br>
여기서 발생한 문제는<br>
메모리의 할당과 해제에는 많은 자원을 필요로 한다는 것입니다.<br>
많은 수의 플레이어들이 동시에 총알을 할당/해제 하다보면 컴퓨터의 성능에 영향을 미칠 것입니다.<br>
<br>
한가지 문제가 더 존재하는데<br>
메모리 단편화가 일어난다는 것입니다. 메모리 단편화는 메모리를 자주 할당/해제 하다보면 나타나는 문제로<br>
메모리 공간이 조각나게 되어 나중에는 할당할 수 있는 연속적인 공간이 부족하게 된다는 것입니다.
> ## 구현

이를 해결하기 위해 오브젝트 풀 방식을 사용할 수 있습니다.<br>
오브젝트 풀 방식이란 풀에 미리 오브젝트를 생성해 놓고 이를 사용함으로 써<br>
메모리 할당/해제를 지양하는 방식을 말합니다.<br>
(간략하게 만들기 위해 소멸자나 delete는 제외했습니다.)
```cpp
class Bullet final {
public:
	bool Active(void) { //총알이 활성 상태인지 확인합니다.
		return _active;
	}
	void Init(void) { //초기화를 진행하는 함수입니다.
		_active = true;
	}
	void Hit(void) { //총알이 부딛혔을시 활성 상태를 끕니다.
		_active = false;
	}
private:
	bool _active = false;
};
```
```cpp
class ObjectPool final {
public:
	void Shot(void) { //총을 쏘라는 명령이 들어오면
		for (auto& iter : _bullets)
			if (false == iter->Active()) { //총알중에 활성 상태가 아닌것을 찾아
				iter->Init(); //초기화를 시킵니다.(발사합니다.)
				return;
			}
	}
	void Hit(void) { //총알이 부딛혔는지 확인합니다.
		for (auto& iter : _bullets)
			iter->Hit();
	}
private:
	std::vector<Bullet*> _bullets{ new Bullet, new Bullet, new Bullet, new Bullet, new Bullet };
};
```
```cpp
class Gun final {
public:
	bool Shot(void) {
		return true;
	}
};
```
```cpp
void main(void) {
	std::vector<Gun*> guns{ new Gun, new Gun, new Gun, new Gun, new Gun };
	ObjectPool obectpool;

	while (true) {
		for (auto& iter : guns)
			if (iter->Shot()) //총을 쏘라는 명령이 들어오면
				obectpool.Shot(); //오브젝트 풀에 총을 쏘라는 명령을 내립니다.

		obectpool.Hit();
	}
};
```
이제 총알은 처음을 제외하고 메모리 할당/해제 되지 않습니다.<br>
오브젝트 풀 패턴을 사용하기 위해 변경된 내용을 확인해 보겠습니다.<br>
<br>
Bullet.<br>
총알은 이제 active라는 변수를 가지게 되었습니다. 이를 통해 이 총알이 사용가능한지 확인합니다.<br>
또한 init 함수를 지니게 되었는데 새로 생성하는것이 아니기 때문에 재사용 할 때마다 초기화가 필요합니다.<br>
마지막으로 hit함수가 호출되면 소멸대신 active를 false로 만듭니다.<br>
<br>
objectpool.<br>
오브젝트 풀이라는 클래스가 추가되었습니다. 이 객체는 총알을 미리 5개 생성해 두었습니다.<br>
Shot함수가 호출 된다면 active되지 않은 총알을 찾아 init을 호출하게 합니다.<br>
hit함수는 총알의 hit 상태를 확인하게 만듭니다.<br>
gun.<br>
더이상 총알을 생성하지 않고 objectpool에게 총알 생성을 요청합니다.<br>

> ## 주의
