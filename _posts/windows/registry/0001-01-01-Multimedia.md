---
title: "멀티미디어(Multimedia)"
categories:
  - registry
tags:
  - tag
---
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia
```
MMCSS(멀티미디어 클래스 스케줄러 서비스)를 사용하면 멀티미디어 애플리케이션에서 시간이 중요한 처리가 CPU 리소스에 대한 우선 순위가 있는 액세스를 받도록 할 수 있습니다. 이 서비스를 사용하면 우선 순위가 낮은 애플리케이션에 대한 CPU 리소스를 거부하지 않고 멀티미디어 애플리케이션에서 가능한 많은 CPU를 활용할 수 있습니다.

MMCSS는 레지스트리에 저장된 정보를 사용하여 지원되는 작업을 식별하고 이러한 작업을 수행하는 스레드의 상대적 우선 순위를 결정합니다. 특정 작업과 관련된 작업을 수행하는 각 스레드는 AvSetMmMaxThreadCharacteristics 또는 AvSetMmThreadCharacteristics 함수를 호출하여 해당 작업에서 작업 중임을 MMCSS에 알릴 수 있습니다.

## 시스템 프로필(SystemProfile)
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile
```
### 네트워크 조절 인덱스(NetworkThrottlingIndex)

|이름|종류|데이터(기본값)|
|---|---|---|
|NetworkThrottlingIndex|REG_DWORD|0x0000000a(10)|

### NoLazyMode

|이름|종류|데이터(기본값)|
|---|---|---|
||REG_DWORD||

### 시스템 응답성(SystemResponsiveness)

|이름|종류|데이터(기본값)|
|---|---|---|
|SystemResponsiveness|REG_DWORD|0x00000014(20)|

이 키에는 우선 순위가 낮은 작업으로 보장되어야 하는 CPU 리소스의 비율을 결정하는 값이 포함되어 있다.  
예를 들어 이 값이 20이면 CPU 리소스의 20%가 우선 순위가 낮은 작업을 위해 예약된다.  
10으로 균등하게 나눌 수 없는 값은 10의 가장 가까운 배수로 반올림된다. 값 0도 10으로 처리된다.

## 작업(Tasks)
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\Tasks\Audio
```
|이름|종류|데이터(기본값)|
|---|---|---|
|Affinity|REG_DWORD|0x00000000(0)|
|Background Only|REG_SZ|True|
|Clock Rate|REG_DWORD|0x00000000(0)|
|GPU Priority|REG_DWORD|0x00000000(0)|
|Latency Sensitive|REG_SZ||
|Priority|REG_DWORD|0x00000000(0)|
|Scheduling Category|REG_SZ||
|SFIO Priority|REG_SZ||


