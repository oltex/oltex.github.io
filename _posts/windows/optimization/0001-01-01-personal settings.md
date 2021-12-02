---
title: "개인 설정"
categories:
  - windows-optimization
tags:
  - tag
---

## Registry
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
"FeatureSettingsOverride"=dword:00000003  
"FeatureSettingsOverrideMask"=dword:00000003

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management\PrefetchParameters]  
"EnablePrefetcher"=dword:00000000 (Default:3)

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Ndu]  
"Start"=dword:00000004 (Default:2)

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\kbdclass\Parameters]  
"KeyboardDataQueueSize"=dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\mouclass\Parameters]  
"MouseDataQueueSize"=dword:00000000

## BCDEdit
bcdedit /set tscsyncpolicy legacy  
bcdedit /set useplatformclock no  
bcdedit /set useplatformtick no  
bcdedit /set disabledynamictick yes

## Powercfg
powercfg -h off  
powercfg -duplicatescheme e9a42b02-d5df-448d-aa00-03f14749eb61

## Netsh
netsh int tcp set global rss=disable  
netsh int tcp set global autotuninglevel=disable

## PowerShell
Get-AppxPackage Microsoft.XboxApp | Remove-AppxPackage  
Get-AppxPackage Microsoft.WindowsSoundRecorder | Remove-AppxPackage  
Get-AppxPackage Microsoft.WindowsAlarms | Remove-AppxPackage  
Get-AppxPackage Microsoft.XboxGameOverlay | Remove-AppxPackage  
Get-AppxPackage Microsoft.MSPaint | Remove-AppxPackage  
Get-AppxPackage Microsoft.Messaging | Remove-AppxPackage  
Get-AppxPackage Microsoft.MicrosoftStickyNotes | Remove-AppxPackage  
Get-AppxPackage Microsoft.Microsoft3DViewer | Remove-AppxPackage  
Get-AppxPackage Microsoft.ZuneMusic | Remove-AppxPackage  
Get-AppxPackage Microsoft.ZuneVideo | Remove-AppxPackage  
Get-AppxPackage Microsoft.WindowsStore | Remove-AppxPackage  
Get-AppxPackage Microsoft.StorePurchaseApp | Remove-AppxPackage  
Get-AppxPackage Microsoft.XboxIdentityProvider | Remove-AppxPackage  
Get-AppxPackage Microsoft.WindowsCamera | Remove-AppxPackage  
Get-AppxPackage Microsoft.Windows.Photos | Remove-AppxPackage  
Get-AppxPackage Microsoft.XboxGamingOverlay | Remove-AppxPackage  
Get-AppxPackage Microsoft.People | Remove-AppxPackage  
Get-AppxPackage Microsoft.XboxSpeechToTextOverlay | Remove-AppxPackage  
Get-AppxPackage Microsoft.Wallet | Remove-AppxPackage  
Get-AppxPackage Microsoft.GetHelp | Remove-AppxPackage  
Get-AppxPackage Microsoft.WindowsMaps  | Remove-AppxPackage  
Get-AppxPackage microsoft.windowscommunicationsapps  | Remove-AppxPackage

## Services
AllJoyn Router Service : 사용 안 함  
App Readiness : 사용 안 함  
Application Identity : 사용 안 함  
Application Layer Gateway Service
AppX Deployment Service (AppXSVC)
AssignedAccessManager 서비스
*AsusUpdateCheck*
AVCTP 서비스
Background Intelligent Transfer Service
BitLocker Drive Encryption Service
Block Level Backup Engine Service
Bluetooth 사용자 지원 서비스_19e1a
Bluetooth 오디오 게이트웨이 서비스
Bluetooth 지원 서비스
BranchCache
CaptureService_19e1a
Certificate Propagation
Client License Service (ClipSVC)
CNG Key Isolation
COM+ Event System
COM+ System Application
Connected User Experiences and Telemetry
Contact Data_19e1a
Data Sharing Service
Device Association Service
DevicePicker_19e1a
DevicesFlow_19e1a
DevQuery Background Discovery Broker
Diagnostic Policy Service
Diagnostic Service Host
Diagnostic System Host
Distributed Link Tracking Client
Distributed Transaction Coordinator
dmwappushsvc
DNS Client
Downloaded Maps Manager
Encrypting File System (EFS)
Extensible Authentication Protocol
File History Service
Function Discovery Provider Host
Function Discovery Resource Publication
GameDVR 및 브로드캐스트 사용자 서비스_19e1a
Geolocation Service
*Google Chrome Elevation Service (GoogleChromeElevationService)*
*Google 업데이트 서비스 (gupdate)*
*Google 업데이트 서비스 (gupdatem)*
GraphicsPerfSvc
HV 호스트 서비스
Hyper-V Data Exchange Service
Hyper-V Guest Service Interface
Hyper-V Guest Shutdown Service
Hyper-V Heartbeat Service
Hyper-V PowerShell Direct Service
Hyper-V Time Synchronization Service
Hyper-V 볼륨 섀도 복사본 요청자
Hyper-V 원격 데스크톱 가상화 서비스
IKE and AuthIP IPsec Keying Modules
Infrared monitor service
Internet Connection Sharing (ICS)
IP Helper
IP 변환 구성 서비스
IPsec Policy Agent
KtmRm for Distributed Transaction Coordinator
Link-Layer Topology Discovery Mapper
MessagingService_19e1a
Microsoft (R) 진단 허브 표준 수집기 서비스
Microsoft Account Sign-in Assistant
Microsoft App-V Client
