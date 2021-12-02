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


[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\FTH]  
"Enabled"=dword:00000000 (Default:1)

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\DataCollection]  
"AllowTelemetry"=dword:00000000 (그룹정책에 있음)  
"DoNotShowFeedbackNotifications"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\TouchPrediction]  
"Latency"=dword:00000002 (Default:8)  
"SampleTime"=dword:00000002 (Default:8)

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile]  
"NetworkThrottlingIndex"=dword:0000000a (Default:10)  
"SystemResponsiveness"=dword:0000000a (Default:20)  
"NoLazyMode"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\Tasks\Audio]  
"Affinity"=dword:00000000  
"Background Only"="True"  
"Clock Rate"=dword:00002710  
"GPU Priority"=dword:00000000  
"Priority"=dword:00000001  
"Scheduling Category"="Low"  
"SFIO Priority"="Low"  
"Latency Sensitive"="False"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\Tasks\Capture]  
"Affinity"=dword:00000000  
"Background Only"="True"  
"Clock Rate"=dword:00002710  
"GPU Priority"=dword:00000000  
"Priority"=dword:00000001  
"Scheduling Category"="Low"  
"SFIO Priority"="Low"  
"Latency Sensitive"="False"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\Tasks\DisplayPostProcessing]  
"Affinity"=dword:00000000  
"Background Only"="True"  
"BackgroundPriority"=dword:00000008  
"Clock Rate"=dword:00002710  
"GPU Priority"=dword:0000001f  
"Priority"=dword:00000008  
"Scheduling Category"="High"  
"SFIO Priority"="High"  
"Latency Sensitive"="True"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\Tasks\Distribution]  
"Affinity"=dword:00000000  
"Background Only"="True"  
"Clock Rate"=dword:00002710  
"GPU Priority"=dword:00000000  
"Priority"=dword:00000001  
"Scheduling Category"="Low"  
"SFIO Priority"="Low"  
"Latency Sensitive"="False"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\Tasks\Games]  
"Affinity"=dword:00000000  
"Background Only"="False"  
"Clock Rate"=dword:00002710  
"Priority"=dword:00000008  
"Scheduling Category"="High"  
"SFIO Priority"="High"  
"GPU Priority"=dword:0000001f  
"Latency Sensitive"="True"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\Tasks\Playback]  
"Affinity"=dword:00000000  
"Background Only"="False"  
"BackgroundPriority"=dword:00000001  
"Clock Rate"=dword:00002710  
"GPU Priority"=dword:00000000  
"Priority"=dword:00000001  
"Scheduling Category"="Low"  
"SFIO Priority"="Low"  
"Latency Sensitive"="False"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\Tasks\Pro Audio]  
"Affinity"=dword:00000000  
"Background Only"="False"  
"Clock Rate"=dword:00002710  
"GPU Priority"=dword:00000000  
"Priority"=dword:00000001  
"Scheduling Category"="Low"  
"SFIO Priority"="Low"  
"Latency Sensitive"="False"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\Tasks\Window Manager]  
"Affinity"=dword:00000000  
"Background Only"="True"  
"Clock Rate"=dword:00002710  
"GPU Priority"=dword:00000000  
"Priority"=dword:00000001  
"Scheduling Category"="Low"  
"SFIO Priority"="Low"  
"Latency Sensitive"="False"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\Maintenance]  
"MaintenanceDisabled"=dword:00000001

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

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\PriorityControl]  
"IQR4294967294Priority"=dword:00000001  
"IQR4294967293Priority"=dword:00000001  
"Win32PrioritySeparation"=dword:00000026 (Default:2)

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management]  
"DisablePagingExecutive"=dword:00000001 (Default:0)



[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Ndu]  
"Start"=dword:00000004 (Default:2)
