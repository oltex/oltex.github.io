---
title: "멀티미디어(Multimedia)"
categories:
  - windows-registry
tags:
  - tag
---
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia
```
MMCSS(멀티미디어 클래스 스케줄러 서비스)를 사용하면 멀티미디어 애플리케이션에서 시간이 중요한 처리가 CPU 리소스에 대한 우선 순위가 있는 액세스를 받도록 할 수 있습니다.  
이 서비스를 사용하면 우선 순위가 낮은 애플리케이션에 대한 CPU 리소스를 거부하지 않고 멀티미디어 애플리케이션에서 가능한 많은 CPU를 활용할 수 있습니다.

MMCSS는 레지스트리에 저장된 정보를 사용하여 지원되는 작업을 식별하고 이러한 작업을 수행하는 스레드의 상대적 우선 순위를 결정합니다.  
특정 작업과 관련된 작업을 수행하는 각 스레드는 AvSetMmMaxThreadCharacteristics 또는 AvSetMmThreadCharacteristics 함수를 호출하여 해당 작업에서 작업 중임을 MMCSS에 알릴 수 있습니다.

## 시스템 프로필(SystemProfile)
`\SystemProfile`
### 네트워크 조절 인덱스(NetworkThrottlingIndex)

|이름|종류|데이터(기본값)|
|---|---|---|
|NetworkThrottlingIndex|REG_DWORD|0x0000000a(10)|

### NoLazyMode

|이름|종류|데이터(기본값)|
|---|---|---|
|NoLazyMode|REG_DWORD||

### 시스템 응답성(SystemResponsiveness)

|이름|종류|데이터(기본값)|
|---|---|---|
|SystemResponsiveness|REG_DWORD|0x00000014(20)|

이 키에는 우선 순위가 낮은 작업으로 보장되어야 하는 CPU 리소스의 비율을 결정하는 값이 포함되어 있다.  
예를 들어 이 값이 20이면 CPU 리소스의 20%가 우선 순위가 낮은 작업을 위해 예약된다.  
10으로 균등하게 나눌 수 없는 값은 10의 가장 가까운 배수로 반올림된다. 값 0도 10으로 처리된다.

## 작업(Tasks)
```
SystemProfile\Tasks
```
### 오디오(Audio) 캡처(Capture) 디스플레이 후처리(DisplayPostProcessing) 배포(Distribution) 게임(Games) 재생(Playback) Pro 오디오(Pro Audio) 창 관리자(Window Manager)
```
\Audio \Capture \DisplayPostProcessing \Distribution \Games \Playback \Pro Audio \Window Manager
```
키에는 작업 목록을 포함하는 Tasks라는 하위 키도 포함되어 있습니다. 기본적으로 Windows는 다음 작업을 지원합니다.

이름|종류|오디오(Audio)|캡처(Capture)|디스플레이 후처리(DisplayPostProcessing)|배포(Distribution)|게임(Games)|재생(Playback)|Pro 오디오(Pro Audio)|창 관리자(Window Manager)
---|---|---|---|---|---|---|---|---|---
Affinity|REG_DWORD|0x00000000(0)|0x00000000(0)|0x00000000(0)|0x00000000(0)|0x00000000(0)|0x00000000(0)|0x00000000(0)|0x00000000(0)
Background Only|REG_SZ|True|True|True|True|False|False|False|True
BackgroundPriority|REG_DWORD|||0x00000008(8)|||0x00000004(4)||
Clock Rate|REG_DWORD|0x00002710(10000)|0x00002710(10000)|0x00002710(10000)|0x00002710(10000)|0x00002710(10000)|0x00002710(10000)|0x00002710(10000)|0x00002710(10000)
GPU Priority|REG_DWORD|0x00000008(8)|0x00000008(8)|0x00000008(8)|0x00000008(8)|0x00000008(8)|0x00000008(8)|0x00000008(8)|0x00000008(8)
Latency Sensitive|REG_SZ||||||||
Priority|REG_DWORD|0x00000006(6)|0x00000005(5)|0x00000008(8)|0x00000004(4)|0x00000002(2)|0x00000003(3)|0x00000001(1)|0x00000005(5)
Scheduling Category|REG_SZ|Medium|Medium|High|Medium|Medium|Medium|High|Medium
SFIO Priority|REG_SZ|Normal|Normal|Normal|Normal|Normal|Normal|Normal|Normal

각 작업 키에는 태스크와 연결된 스레드에 적용할 특성을 나타내는 다음 값 집합이 포함되어 있습니다.

이름|종류|가능한 데이터|
---|---|---|
Affinity|REG_DWORD|프로세서 선호도를 나타내는 비트맵입니다. 0x00 및 0xFFFFFFFF 모두 프로세서 선호도가 사용되지 않음을 나타냅니다.
Background Only|REG_SZ|이 작업이 백그라운드 작업(사용자 인터페이스 없음)인지 여부를 나타냅니다. 창 포커스가 변경되어 백그라운드 작업의 스레드가 변경되지 않습니다. 이 값은 True 또는 False로 설정할 수 있습니다.
BackgroundPriority|REG_DWORD|백그라운드 우선 순위입니다. 값의 범위는 1~8입니다.
Clock Rate|REG_DWORD|프로세서 리소스 예약의 세분성을 확인하기 위해 MMCSS에서 사용하는 힌트입니다. Windows Server 2008 및 Windows Vista: 스레드가 이 작업에 조인하는 경우 시스템에서 사용하는 최대 보장 클록 속도(100나노초 간격)입니다. Windows 7 및 Windows Server 2008 R2부터 시스템 전원 소비를 줄이기 위해 이 보장이 제거되었습니다.
GPU Priority|REG_DWORD|GPU 우선 순위입니다. 값의 범위는 0-31입니다. 이 우선 순위는 아직 사용되지 않습니다.
Latency Sensitive|REG_SZ|
Priority|REG_DWORD|작업 우선 순위입니다. 값의 범위는 1(낮음)~8(높음)입니다. 예약 범주가 High인 작업의 경우 이 값은 항상 2로 처리됩니다.
Scheduling Category|REG_SZ|예약 범주입니다. 이 값은 High, Medium 또는 Low으로 설정할 수 있습니다.
SFIO Priority|REG_SZ|예약된 I/O 우선 순위입니다. 이 값은 Idle, Low, Normal 또는 High로 설정할 수 있습니다. 이 값은 사용되지 않습니다.
