---
title: "프로세서 사용 계획(Win32PrioritySeparation)"

categories:
  - registry
tags:
  - tag
---
```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\PriorityControl
```

|이름|종류|데이터(기본값)|
|---|---|---|
|Win32PrioritySeparation|REG_DWORD|0x00000002(2)|

윈도우즈 시스템 성능 옵션에가면 '프로세서 사용 계획' 항목으로  
'다음의 최적 성능을 위해 조정' 프로그램 \| 백그라운드 선택하게 되어있다.

쉽게 설명하면 어플리케이션이 종종 렉을 유발하는경우 포그라운드로 설정하면 나아진다.  
백그라운드앱이 실행되면서 프로세서를 사용하게되면 종종 렉을 유발하기 때문

세부적인 세팅벨류는 비트자리로 구분된다.

|||||
|---|---|---|---|
|Quantum Long, Short|01XXXX|10XXXX||
|Interval Fixed, Variable|XX10XX|XX01XX||
|Ratio 1:1, 2:1, 3:1|XXXX00|XXXX01|XXXX10, XXXX11|

**포그라운드(Foreground) VS 백그라운드(Background)**  
윈도우 성능 설정에서는 SHORT LONG 기준으로 포그라운드 또는 백그라운드로 표시된다.  
사실에서 어긋난 설정이므로 주의해야한다.  
포그라운드 설정의경우 현재 입력 쓰레드로 사용중인 프로그램에 기준으로 CPU를 부여해준다.

고정(Fixed) VS 가변(Variable)  
포그라운드 기준 세팅이다. 이 설정은 포그라운드에  비율을 고정 또는 가변으로 처리해준다.  

비율(Ratio) 1:1 VS 2:1 VS 3:1  
비율 세팅이다. 이 설정은 포그라운드 기준으로 몇 회의 CPU를 할당 하는지를 결정한다.
