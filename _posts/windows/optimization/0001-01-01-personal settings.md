---
title: "개인 설정"
categories:
  - windows-optimization
tags:
  - tag
---

**원격 분석 허용** 
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\DataCollection
AllowTelemetry : 0

|이름|종류|데이터(기본값)|
|---|---|---|
|AllowTelemetry|REG_DWORD||

 : 0
그룹정책에 있음

**Windows에서 내 피드백 요청 안 함**

|이름|종류|데이터(기본값)|
|---|---|---|
|DoNotShowFeedbackNotifications|REG_DWORD||

권장 : 1

```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\FTH
```

|이름|종류|데이터(기본값)|
|---|---|---|
|Enabled|REG_DWORD|0x00000001(1)|

권장 : 0

```
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer
```
**알트 탭 설정**

|이름|종류|데이터(기본값)|
|---|---|---|
|AltTabSettings|REG_DWORD||

권장 : 1
