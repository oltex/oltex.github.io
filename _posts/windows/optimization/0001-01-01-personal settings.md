---
title: "개인 설정"
categories:
  - windows-optimization
tags:
  - tag
---

## 레지스트리
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer]
"AltTabSettings"=dword:00000001  

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\DataCollection]
"AllowTelemetry"=dword:00000000 (그룹정책에 있음)  
"DoNotShowFeedbackNotifications"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\FTH]
"Enabled"=dword:00000000 (Default : 1)

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management]
"DisablePagingExecutive"=dword:00000001 (Default : 1)

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Photo Viewer\Capabilities\FileAssociations]
".tif"="PhotoViewer.FileAssoc.Tiff"  
".tiff"="PhotoViewer.FileAssoc.Tiff"  
".bmp"="PhotoViewer.FileAssoc.Tiff"  
".dib"="PhotoViewer.FileAssoc.Tiff"  
".gif"="PhotoViewer.FileAssoc.Tiff"  
".jfif"="PhotoViewer.FileAssoc.Tiff"  
".jpe"="PhotoViewer.FileAssoc.Tiff"  
".jpeg"="PhotoViewer.FileAssoc.Tiff"  
".jpg"="PhotoViewer.FileAssoc.Tiff"  
".jxr"="PhotoViewer.FileAssoc.Tiff"  
".png"="PhotoViewer.FileAssoc.Tiff"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power]
"ExitLatency"=dword:00000001  
"ExitLatencyCheckEnabled"=dword:00000001  
"Latency"=dword:00000001  
"LatencyToleranceDefault"=dword:00000001  
"LatencyToleranceFSVP"=dword:00000001  
"LatencyTolerancePerfOverride"=dword:00000001  
"LatencyToleranceScreenOffIR"=dword:00000001  
"LatencyToleranceVSyncEnabled"=dword:00000001  
"RtlCapabilityCheckLatency"=dword:00000001

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