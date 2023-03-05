---
title: "빅-오 표기법(Big-Oh Natation)"
categories:
  - algorithm
tags:
  - tag
---
> ## 개요

알고리즘의 효율성을 따지기 위한 표기법으로,<br>
시간 복잡도를 점근 표기법으로 나타냅니다.
> ## 사전 지식

### 시간 복잡도
(빅오 표기법을 설명하기 위해서는<br>
시간 복잡도에 대한 사전 지식이 필요합니다.)

### 점근 표기법(asymptotic notation)
개요에서 설명하기를<br>
시간 복잡도를 점근 표기법 나타내는 방법이라고 하였습니다.<br>
<br>
먼저 점근적이라는 뜻을 알아보겠습니다.<br>
<br>
사전적 의미로 점근이란 점점 가까워짐을 의미하는 말입니다.<br>
그렇다면 무엇에 가까워 짐을 의미하는 것일까요.<br>
수학적 의미로 점근이란 변수 또는 함수가 한계치(보통 무한대)에 가까워짐을 의미합니다.<br>
그리고 그에 따른 값에 접근한다는 의미이기도 합니다.<br>
<br>
점근 표기법은 이러한 점근의 특성을 이용합니다.<br>
시간 복잡도 함수를 무한대로 보내서 표기하겠다.는 뜻을 의미합니다.<br>
<br>
이를 수학적 언어로 표현하면<br>
시간복잡도를 근사치(approximation)로 표현하겠다. 라는 뜻이 됩니다.<br>
<br>
이러한 점근 표기법을 사용하는 이유는<br>
함수가 있을때, 보다 간단한 함수로 만들어 표시하기 위함입니다.<br>
<br>
이러한 점근 표기법에는 여러가지가 존재합니다.<br>
그중 가장 많이쓰이고 있는 빅오 표기법에 대해 설명하겠습니다.
> ## 설명

이러한 시간 복잡도 함수가 존재한다고 가정해보겠습니다.
```
T(n) = n^2 + 2n + 1
```
사실 시간 복잡도 함수는 정확히 구하는 과정이 녹록지 않습니다.<br>
따라서 다음과 같은 고민을 할 수 있습니다.<br>
<br>
이 시간 복잡도 함수는 n^2 + 2n + 1 아니면 n^2 + n + 1일텐데...<br>
이러한 고민을 빅오 표기법을 통해 해결해 보겠습니다.

### 빅오 표기법(점근)
빅오 표기법은 함수에서 가장 영향력이 큰 부분을 표기하는 방식입니다.<br>
영어로 얘기하자면 Big O 즉 가장 큰 O를 따지는 것입니다. (여기서 O는 영향력을 의미합니다.)<br>
<br>
시간 복잡도에서 차지하는 가장 영향력이 큰 부분을 제외한 모든 부분을 날려버립니다.<br>
한번 T(n) = n^2 + 2n + 1에서 각 부분이 차지하는 비율을 확인해 보겠습니다.

n|n^2|2n|1|n^2의 비율
---|---|---|---|---
10|100|20|1|82.64%
100|10000|200|1|98.03%
1000|1000000|2000|1|99.80%

n이 조금만 증가하여도 n^2가 차지하는 비율이 98%를 넘어서는 모습을 확인할 수 있습니다.<br>
n이 증가할 수록 2n + 1가 미치는 영향은 미미해집니다.<br>
따라서 다음과 같이 간략화 할 수 있습니다.
```
T(n) = n^2
```
이를 빅오 표기법으로 표시하면 이렇습니다.
```
O(n^2)
```
### 빅오 표기법(증감)

이제 가장 영향력이 큰 부분을 찾는 것까진 알았습니다.<br>
다음과 같은 시간 복잡도가 있을 때 빅오 표기법은 어떻게 될까요?
```
T(n) = n^2

T(n) = 100n^2
```
두 시간 복잡도의 빅오 표기법은 다음과 같습니다.
```
O(n^2)

O(n^2)
```
빅오 표기법은 가장 영향력이 큰 부분만 남기는 것은 알고있는데<br>
100은 왜 제거됐을까요?<br>
<br>
이는 빅오 표기법의 또다른 의미에서 찾아볼 수 있습니다.<br>
빅오 표기법은 데이터 수의 증가에 따른 연산 횟수의 증가 형태를 나타내는 표기법이기도 합니다.<br>
<br>
즉 빅오의 관점에서 두 함수는 동일하다고 볼 수 있습니다.
- 데이터 수가 2, 3, 4개로 늘어날 때마다 연산횟수는 4, 8, 16으로 두 배씩 늘어났습니다.
- 데이터 수가 2, 3, 4개로 늘어날 때마다 연산횟수는 99, 198, 396으로 두 배씩 늘어났습니다.

결국 빅오를 이해하는 방향은<br>
함수의 증감 추세를 보기 위한 표기법이라는 것입니다.<br>
<br>
따라서 다음이 의미하는 바는
```
O(logn)

데이터 수의 증가에 따른 연산 횟수의 증가 형태를 좌표평면상에 그려놓으면
증가하는 추세가 둔화되는 형태를 띤다. 즉 로그 그래프와 유사한 형태를 띤다.
```
라고 이해하는 것이 옳습니다.