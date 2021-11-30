---
title: "IQR 우선순위(IQRPriority)"

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
|IQR`?`Priority|REG_DWORD|0x00000000(0)|


간혹 일부 주변기기가 인터럽트 핸들러에서 오래 홀드하는 경우, 사운드 카드 등이 우선 순위에 밀려 문제가 일어나는 경우가 있을 수 있습니다.  
일반적인 경우가 아닌 구형 LAN 카드나 HDTV 카드 등 사용시 문제가 일어나는 경우는 아래 그림과 같이 사운드 카드의 IRQ 우선 순위를 올려주시기 바랍니다.  

먼저 시작 -> 실행 -> 시스템 정보(msinfo32)를 실행합니다.

현재 사용하는 IRQ가 몇번인지 찾습니다.

다시 시작 -> 실행 -> REGEDIT에서 아래로 순서대로 이동합니다.  그 다음 마우스 오른쪽 버튼을 눌러서 DWORD를 선택하시고.. 
IRQ18Priority를 입력합니다. (IRQ가 10번이면 IRQ10Priority를 하면 되겠지요)

[HKEY_LOCAL_MACHINE -> System -> CurrentControlSet -? Control -> PriorityControl]

그리고 값을 1로 주고 재부팅하면 IRQ 18번이 우선순위를 가지게 되어 만약 우선 순위에 밀려서 일어나는 문제가 있었다면 어느 정도 효과를 볼 수 있습니다.
