---
title: "프로세서 사용 계획(Win32PrioritySeparation)"

categories:
  - registry
tags:
  - tag
---
경로
```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\PriorityControl
```

|이름|종류|데이터(기본값)|
|---|---|---|
|Win32PrioritySeparation|REG_DWORD|0x00000002(2)|

윈도우즈 시스템 성능 옵션에가면 '프로세서 사용 계획' 항목으로 '다음의 최적 성능을 위해 조정' 프로그램 \| 백그라운드 선택하게 되어있다.

쉽게 설명하면 어플리케이션이 종종 렉을 유발하는경우 포그라운드로 설정하면 나아진다.
백그라운드앱이 실행되면서 프로세서를 사용하게되면 종종 렉을 유발하기 때문

세부적인 세팅벨류는 비트자리로 구분된다.

기본값
2:DEFAULT
||Win32PrioritySeparation|SETTING|
||||
||||
