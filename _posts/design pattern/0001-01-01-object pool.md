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
<br>
gun.<br>
더이상 총알을 생성하지 않고 objectpool에게 총알 생성을 요청합니다.<br>
> ## 주의

오브젝트 풀 사용시 다음과 같은 주의 사항을 확인해야 합니다.
### 메모리
오브젝트 풀은 메모리의 할당/해제를 직접 관리하기 위한 패턴입니다.<br>
따라서 오브젝트 풀의 크기를 조절하는 것이 중요합니다.<br>
<br>
만약 오브젝트 풀이 너무 작다면 오브젝트 생성에 문제가 발생할 것입니다.<br>
반대로 오브젝트 풀을 너무 크게 설정한다면 메모리를 낭비한다는 소리가 됩니다.<br>
### 예외
앞서 말했듯이 오브젝트 풀의 크기를 잘 조정했다 하더라도<br>
이는 예측에 의한 것이기 때문에 오브젝트 풀의 개수를 초과해 요청이 들어올 경우가 존재할 것입니다.<br>
<br>
모든 오브젝트가 active 상태인데 create요청이 들어올 경우를 대비하여야합니다.<br>
이에 대해 몇가지 선택지가 존재합니다.
- 객체를 생성하지 않습니다: 총알이 전부 활성 상태라면 총알을 생성하지 않습니다.
- 기존 객체를 제거합니다: 총알이 전부 활성 상태라면 가장 먼저 나간(오래된) 총알을 초기화해 반환합니다.
- 풀의 크기를 늘립니다: 게임에서 좀더 유연한 대처를 위해 런타임에 풀의 크기를 늘려줍니다.
- 이런 일이 아예 생기지 않게 합니다: 위 예시랑 맞지 않긴 하지만 어떤 오브젝트 풀의 경우 최대치를 예측할 수 있습니다.<br>
예를 들어 총알의 경우 플레이어당 100발이 주어진다면 플레이어 x 100 으로 총알의 최대치를 예측 가능합니다.

### 커플링
구현에서 사용한 코드는 오브젝트풀과 총알 클래스 사이에<br>
커플링이 존재하는 것을 확인 할 수 있습니다.<br>
오브젝트 풀에서 총알의 생성과 반환 뿐아니라, 총알의 제어 함수도 들어있다는 것입니다.<br>
<br>
이런 방식과 다르게 두 클래스를 디커플링 시키는 방법도 존재합니다.<br>
오브젝트 풀은 총알의 생성과 반환에만 집중하고 총알의 제어는 외부에서 하는 것입니다.
### 탐색
구현 코드를 보면 사용 가능한 총알을 찾기 위해 탐색을 진행하는 것을 볼 수 있습니다.<br>
오브젝트 풀이 작다면 문제없겠지만, 만약 크다면 할당/해제보다 탐색에 드는 비용이 더 많을 수 있습니다.<br>
<br>
이 문제를 해결하기 위해 몇가지 방법이 존재합니다.
#### 별도의 컬렉션
컬렉션을 추가하는 방법입니다. 오브젝트 풀에 활성 상태가 아닌 컬렉션을 제공합니다.
```cpp
class ObjectPool final {
public:
	void Shot(void) {
		if (_unactive_bullets.empty())
			return;
		Bullet* bullet = _unactive_bullets.front();
		_unactive_bullets.pop_front();

		bullet->Init();
		_active_bullets.emplace_back(bullet);
	}
	void Hit(void) {
		for (auto iter = _active_bullets.begin(); iter != _active_bullets.end();) {
			if ((*iter)->Hit()) {
				_unactive_bullets.emplace_back(*iter);
				iter = _active_bullets.erase(iter);
			}
			else
				++iter;
		}
	}
private:
	std::list<Bullet*> _active_bullets; //코드가 복잡하니 쉽게 말하자면, 사용중인 총알은 여기에
	std::list<Bullet*> _unactive_bullets{ new Bullet, new Bullet, new Bullet, new Bullet, new Bullet }; //사용안하는 총알은 여기에 보관하는 방법입니다.
};
```
이렇게 하면 bullet에는 active변수도 필요하지 않습니다.<br>
이방식의 단점은 리스트의 push,pop이 자주 이루어진다는 것입니다.<br>
만약 배열로 만든다 하더라도 기존에 필요했던 포인터 공간의 2배가 필요할 것입니다.
#### 빈칸 리스트
메모리를 최대한 아끼면서 탐색도 하고싶지 않다고 한다면 빈칸 리스트 기법을 사용할 수 있습니다.<br>
먼저 각 Bullet들이 다음 Bullet을 가리키는 포인터를 작성합니다.<br>
<br>
이 방식은 union 키워드를 사용합니다.<br>
추가 예정
