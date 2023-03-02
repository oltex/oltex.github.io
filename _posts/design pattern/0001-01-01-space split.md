---
title: "공간 분할"
categories:
  - design pattern
tags:
  - tag
---
> ## 개요

공간 분할 디자인 패턴입니다.<br>
객체의 효과적인 탐색을 위해,<br>
객체의 위치를 기반으로 공간을 분할하여 자료구조에 저장합니다.<br>
> ### 필요성

게임의 요소중에는 공간이라는 개념이 자리잡고 있습니다.<br>
오브젝트는 이 공간안의 어느 위치에 존재하게 됩니다.<br>
<br>
게임에는 이 공간과 위치를 사용하여 구현되야 하는 로직들이 존재합니다.<br>
가장 대표적인 것이 위치를 기반으로 플레이어의 충돌과 물리 이지만<br>
<br>
조금더 간단한 예시도 있습니다.<br>
특정 오브젝트에서 난 사운드는 주변에만 들려야 할 것이며<br>
주변 범위의 동료에게 버프를 주는 기능등이 있을 것입니다.<br>
<br>
이를 위해서는 주변에 어떤 객체들이 있는지를 탐색해야 합니다.<br>
허나 이는 병목 현상을 유발할 수 있습니다.<br>
<br>
아래는 유닛이 버프를 사용하는 예시입니다.<br>
(간략하게 표현하기 위해 소멸자나 delete는 제외하였습니다.)
```cpp
class Unit final {
private:
	int _pos[2]{ std::rand() % 100, std::rand() % 100 }; //유닛은 0~99의 랜덤한 위치를 가집니다.
};
```
```cpp
void main(void) {
	srand(static_cast<unsigned int>(time(NULL)));

	std::vector<Unit*> units;
	for (int i = 0; i < 100; ++i)
		units.emplace_back(new Unit); //유닛을 100마리 생성합니다.

	for (size_t i = 0; i < units.size(); ++i)
		for (size_t j = 0; j < units.size(); ++j) {
			//만약 두 위치가 가깝다면
			//버프를 발동합니다.
		}
};
```
Unit 100마리를 생성하였습니다.<br>
유닛은 x: 0~99, y: 0~99 사이의 랜덤한 위치를 가집니다.<br>
모든 유닛은 주변에 유닛이 존재하면 버프를 발동합니다.<br>
<br>
이 코드에서 100마리의 유닛이 버프를 확인하기 위해<br>
총 10000번 즉 제곱에 해당하는 탐색을 진행한다는 것을 볼 수 있습니다.
> ## 구현

이러한 오버헤드를 막기 위해 공간을 분할하여 보겠습니다.<br>
이번 구현에서는 가장 간단한 고정 격자(fixed grid)방법을 사용하겠습니다.
```cpp
class Unit final {
public:
	int* GetPos(void) {
		return _pos;
	}
private:
	int _pos[2]{ std::rand() % 100, std::rand() % 100 };
};
```
```cpp
void main(void) {
	srand(static_cast<unsigned int>(time(NULL)));

	std::vector<Unit*> units;
	for (int i = 0; i < 100; ++i)
		units.emplace_back(new Unit);

	std::vector<Unit*> grid[10][10]; //한칸의 길이가 10인 격자를 만듭니다.(0~9, 10~19...)
	for (int i = 0; i < 100; ++i) {
		int* pos = units[i]->GetPos();
		grid[pos[0] % 10][pos[1] % 10].emplace_back(units[i]); //격자에 맞춰 유닛을 집어넣습니다.
	}

	for (size_t i = 0; i < units.size(); ++i) {
		int* pos = units[i]->GetPos();
		int x = pos[0] % 10;
		int y = pos[1] % 10;
		for (size_t j = 0; j < grid[x][y].size(); ++j) { //이제 유닛은 자신이 해당하는 격자만 소통합니다.
			//만약 두 위치가 가깝다면
			//버프를 줍니다.
		}
	}
};
```
정말 간단하게 구현하였습니다.<br>
유닛을 격자에 넣고 해당 유닛이 속한 격자와 상호작용하게 만들었습니다.<br>
<br>
시간복잡도는 마찬가지로 On(2)이지만,<br>
검사 횟수를 카운트 해보면 10000번에서 대략 200번(150~250)으로 크게 줄은것을 확인할 수 있습니다.<br>
<br>
허나 이 코드는 아직 문제가 존재합니다.<br>
예를 들어 어떤 유닛이 (10, 0) 위치에 존재했다고 해보겠습니다.<br>
그리고 다음 유닛이(9, 0)에 존재합니다.<br>
<br>
두 유닛은 가깝기 때문에 버프를 활성화해야 하지만,<br>
격자 상으로는 다른 격자에 속해 있기 때문에 버프를 활성화 시키지 않습니다.<br>
이러한 문제를 해결하기 위해서 주변 격자들도 검사하게 만들어야 합니다.<br>
<br>
(가장 아래쪽 for문을 바꿔줍니다.)
```cpp
	for (size_t i = 0; i < units.size(); ++i) {
		int* pos = units[i]->GetPos();
		int x = pos[0] % 10;
		int y = pos[1] % 10;

		int min[2];
		min[0] = x > 0 ? x - 1 : x;
		min[1] = y > 0 ? y - 1 : y;

		int max[2];
		max[0] = x < 9 ? x + 1 : x;
		max[1] = y < 9 ? y + 1 : y;

		for (int j = min[0]; j <= max[0]; ++j)
			for (int k = min[1]; k <= max[1]; ++k)
				for (size_t j = 0; j < grid[j][k].size(); ++j) {
					//만약 두 위치가 가깝다면
					//버프를 줍니다.
					count++;
				}
	}
```
조금 많이 추가된 것 같지만 정리해보면 주변 8칸을 추가로 검사하게 만들었습니다.<br>
검사 횟수도 1000번(500~1500)정도로 증가하였지만 아직 기존에 비하면 1/10입니다.
