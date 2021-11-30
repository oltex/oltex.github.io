---
title: "프로세서 사용 계획(Win32PrioritySeparation)"

categories:
  - registry
tags:
  - tag
---

## 프로세서 사용 계획(Win32PrioritySeparation)
```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\PriorityControl
```

|이름|종류|데이터(기본값)|
|---|---|---|
|Win32PrioritySeparation|REG_DWORD|0x00000002(2)|

윈도우즈 시스템 성능 옵션에가면 '프로세서 사용 계획' 항목으로  
'다음의 최적 성능을 위해 조정' 프로그램 \| 백그라운드 선택하게 되어있다.

||||
|---|---|---|
|프로그램|0x26(38)|SHORT VARIABLE 3:1|
|백그라운드 서비스|0x18(24)|LONG FIXED 1:1|

쉽게 설명하면 어플리케이션이 종종 렉을 유발하는경우 포그라운드로 설정하면 나아진다.  
백그라운드앱이 실행되면서 프로세서를 사용하게되면 종종 렉을 유발하기 때문

세부적인 세팅벨류는 비트자리로 구분된다.

|||||
|---|---|---|---|
|Quantum Long, Short|01XXXX|10XXXX||
|Interval Fixed, Variable|XX10XX|XX01XX||
|Ratio 1:1, 2:1, 3:1|XXXX00|XXXX01|XXXX10, XXXX11|

**퀀텀 길게(Long) VS 짧게(Short)**  
스레드가 CPU를 얼마 동안 사용할지를 정의한 시간 단위를 퀀텀(Quantum)이라고 한다.  
퀀텀을 길게 설정하면 (36, 대략 12 Click)의 퀀텀을 가지고,  
짧게 설정하면 (6, 대략 2 Click)의 퀀텀을 가진다.

**고정(Fixed) VS 가변(Variable)**  
퀀텀의 실행 시간을 설정한다.  
고정 - 고정된 실행 시간을 가진다. 이 경우 3단계 값은 무시한다.  
가변 - 실행시간이 3단계 설정에 따라 달라진다.

**비율(Ratio) 1:1 VS 2:1 VS 3:1**  
비율 세팅이다. 이 설정은 포그라운드 기준으로 몇 회의 CPU를 할당 하는지를 결정한다.  
1:1 - 포그라운드와 백그라운드 프로그램에 같은 시간을 할당한다.
2:1 - 포그라운드에 2배 많은 시간을 할당한다.  
3:1 - 포그라운드에에 3배 많은 시간을 할당한다.
