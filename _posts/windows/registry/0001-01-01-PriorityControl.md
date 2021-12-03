---
title: "우선 순위 제어(PriorityControl)"
categories:
  - windows registry
tags:
  - tag
---
```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\PriorityControl
```
### IQR 우선순위(IQRPriority)

|이름|종류|데이터(기본값)|
|---|---|---|
|IQR`?`Priority|REG_DWORD||


간혹 일부 주변기기가 인터럽트 핸들러에서 오래 홀드하는 경우, 사운드 카드 등이 우선 순위에 밀려 문제가 일어나는 경우가 있을 수 있습니다.  
일반적인 경우가 아닌 구형 LAN 카드나 HDTV 카드 등 사용시 문제가 일어나는 경우는 아래 그림과 같이 사운드 카드의 IRQ 우선 순위를 올려주시기 바랍니다.  

먼저 시작 -> 실행 -> 시스템 정보(msinfo32)를 실행합니다.

현재 사용하는 IRQ가 몇번인지 찾습니다.

다시 시작 -> 실행 -> REGEDIT에서 아래로 순서대로 이동합니다.  그 다음 마우스 오른쪽 버튼을 눌러서 DWORD를 선택하시고.. 
IRQ18Priority를 입력합니다. (IRQ가 10번이면 IRQ10Priority를 하면 되겠지요)

[HKEY_LOCAL_MACHINE -> System -> CurrentControlSet -? Control -> PriorityControl]

그리고 값을 1로 주고 재부팅하면 IRQ 18번이 우선순위를 가지게 되어 만약 우선 순위에 밀려서 일어나는 문제가 있었다면 어느 정도 효과를 볼 수 있습니다.

### 프로세서 사용 계획(Win32PrioritySeparation)

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
