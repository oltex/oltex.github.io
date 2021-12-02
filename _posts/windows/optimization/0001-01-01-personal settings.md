---
title: "개인 설정"
categories:
  - windows-optimization
tags:
  - tag
---

## 레지스트리

HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer  
AltTabSettings : 1

HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\DataCollection  
AllowTelemetry : 0 (그룹정책에 있음)  
DoNotShowFeedbackNotifications : 1

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\FTH  
Enabled : 0 (Default : 1)

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management  
DisablePagingExecutive : 1 (Default : 1)

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GraphicsDrivers\Power]
"DefaultD3TransitionLatencyActivelyUsed"=dword:00000001  
"DefaultD3TransitionLatencyIdleLongTime"=dword:00000001  
"DefaultD3TransitionLatencyIdleMonitorOff"=dword:00000001  
"DefaultD3TransitionLatencyIdleNoContext"=dword:00000001  
"DefaultD3TransitionLatencyIdleShortTime"=dword:00000001  
"DefaultD3TransitionLatencyIdleVeryLongTime"=dword:00000001  
"DefaultLatencyToleranceIdle0"=dword:00000001  
"DefaultLatencyToleranceIdle0MonitorOff"=dword:00000001  
"DefaultLatencyToleranceIdle1"=dword:00000001  
"DefaultLatencyToleranceIdle1MonitorOff"=dword:00000001  
"DefaultLatencyToleranceMemory"=dword:00000001  
"DefaultLatencyToleranceNoContext"=dword:00000001  
"DefaultLatencyToleranceNoContextMonitorOff"=dword:00000001  
"DefaultLatencyToleranceOther"=dword:00000001  
"DefaultLatencyToleranceTimerPeriod"=dword:00000001  
"DefaultMemoryRefreshLatencyToleranceActivelyUsed"=dword:00000001  
"DefaultMemoryRefreshLatencyToleranceMonitorOff"=dword:00000001  
"DefaultMemoryRefreshLatencyToleranceNoContext"=dword:00000001  
"Latency"=dword:00000001  
"MaxIAverageGraphicsLatencyInOneBucket"=dword:00000001  
"MiracastPerfTrackGraphicsLatency"=dword:00000001  
"MonitorLatencyTolerance"=dword:00000001  
"MonitorRefreshLatencyTolerance"=dword:00000001  
"TransitionLatency"=dword:00000001  
