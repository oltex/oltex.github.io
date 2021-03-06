---
title: "개인 설정"
categories:
  - windows optimization
tags:
  - tag
---

## Registry
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\USB\VID_056A&PID_030E\5&38e97a59&0&1\Device Parameters]  
"DeviceSelectiveSuspended"=dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\USB\VID_056A&PID_030E&MI_00\6&aeb242b&0&0000\Device Parameters]  
"AllowIdleIrpInD3"=dword:00000000  
"SelectiveSuspendEnabled"=hex:00  
"EnhancedPowerManagementEnabled"=dword:00000000  
"LegacyTouchScaling"=dword:00000000  
"WriteReportExSupported"=dword:00000000  
"DeviceResetNotificationEnabled"=dword:00000000  
"VendorRevision"=dword:00000000  
"RevisionId"=dword:00000100  
"ExtPropDescSemaphore"=dword:00000000  
"SelectiveSuspendOn"=dword:00000000

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\csrss.exe\PerfOptions]  
"CpuPriorityClass"=dword:00000004

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

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\USB\AutomaticSurpriseRemoval]  
"AttemptRecoveryFromUsbPowerDrain"=dword:00000000 (Default : 1)

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
bcdedit /set disableelamdrivers yes

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

Get-AppxPackage -Allusers *edge*
Add-AppxPackage -register "C:\Windows\SystemApps\Microsoft.MicrosoftEdge_8wekyb3d8bbwe\AppxManifest.xml" -DisableDevelopmentMode

## Inter(R) Ethernet Connection (7) 1219-V
IPv4 체크섬 오프로드 : Rx & Tx 활성화됨  
RME 활성화 : 비활성화됨  
RSS 로드 밸런싱 프로파일 : 보수적 조정  
TCP 체크섬 오프로드(IPv4) : Rx & Tx 활성화됨  
TCP 체크섬 오프로드(IPv6) : Rx & Tx 활성화됨  
UDP 체크섬 오프로드(IPv4) : Rx & Tx 활성화됨  
UDP 체크섬 오프로드(IPv6) : Rx & Tx 활성화됨  
Wake on 매직 패킷 : 비활성화됨  
Wake on 패턴 일치 : 비활성화됨  
기가비트 매스터 슬레이브 모드 : 강제 슬레이브 모드  
대형 전송 오프로드 V2(IPv4) : 활성화됨  
대형 전송 오프로드 V2(IPv6) : 활성화됨  
레거시 스위치 호환 모드 : 비활성화됨  
로컬 관리 주소 : 없음  
링크 대기 : 오프  
링크 상태 이벤트 기록 : 비활성화됨  
링크 설정 시 깨우기 : 비활성화됨  
링크 속도 배터리 절전 : 활성화됨  
속도 및 이중 : 100Mbps 전이중  
수신 버퍼 : 80
수신측 배율 : 비활성화됨
시스템 휴지 절전기 : 활성화됨  
에너지 효율적인 이더넷 : 온  
인터럽트 조절 : 활성화됨  
인터럽트 조절 속도 : 최대  
적응 프레임간 간격 : 활성화됨  
전송 버퍼 : 80  
전원 차단 시 속도 감소 : 활성화됨  
점보 패킷 : 비활성화됨  
초저전력 모드 : 활성화됨  
최대 RSS 대기열 수 : 1개의 대기열  
패킷 우선순위 및 VLAN : 패킷 우선순위 및 VLAN 비활성화됨  
프로토콜 ARP 오프로드 : 활성화됨  
프로토콜 NS 오프로드 : 활성화됨  
흐름제어 : Rx & Tx 활성화됨

## Services
사용 안 함
AllJoyn Router Service
App Readiness
Application Identity
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
Microsoft iSCSI Initiator Service
Microsoft Passport
Microsoft Passport Container
Microsoft Software Shadow Copy Provider
Microsoft Storage Spaces SMP
Microsoft Store 설치 서비스
Microsoft Windows SMS 라우터 서비스
Network Connected Devices Auto-Setup
Network Connection Broker
Network Connectivity Assistant
Offline Files
Peer Name Resolution Protocol
Peer Networking Grouping
Peer Networking Identity Manager
Performance Counter DLL Host
Performance Logs & Alerts
Phone Service
PNRP Machine Name Publication Service
Print Spooler
Printer Extensions and Notifications
PrintWorkflow_19e1a
Problem Reports and Solutions Control Panel Support
Program Compatibility Assistant Service
Quality Windows Audio Video Experience
Remote Access Auto Connection Manager
Remote Access Connection Manager
Remote Desktop Configuration
Remote Desktop Services
Remote Desktop Services UserMode Port Redirector
Remote Procedure Call (RPC) Locator
Remote Registry
Routing and Remote Access
Secure Socket Tunneling Protocol Service
Security Center
Sensor Data Service
Sensor Monitoring Service
Sensor Service
Server
Shared PC Account Manager
Shell Hardware Detection
Smart Card
Smart Card Device Enumeration Service
Smart Card Removal Policy
SNMP Trap
Spot Verifier
SSDP Discovery
Still Image Acquisition Events
Superfetch
System Event Notification Service
System Guard 런타임 모니터 브로커
TCP/IP NetBIOS Helper
Telephony
Themes
Touch Keyboard and Handwriting Panel Service
Update Orchestrator Service
UPnP Device Host
User Data Access_19e1a
User Data Storage_19e1a
User Experience Virtualization 서비스
Volume Shadow Copy
WalletService
WarpJITSvc
WebClient
Wi-Fi Direct 서비스 연결 관리자 서비스
Windows Biometric Service
Windows Connect Now - Config Registrar
Windows Defender Advanced Threat Protection Service
Windows Defender Antivirus Network Inspection Service
Windows Defender Antivirus Service
Windows Defender Firewall
Windows Defender 보안 센터 서비스
Windows Encryption Provider Host Service
Windows Error Reporting Service
Windows Event Collector
Windows Image Acquisition (WIA)
Windows Perception 서비스
Windows Push Notifications User Service_19e1a
Windows PushToInstall 서비스
Windows Remote Management (WS-Management)
Windows Search
Windows Time
Windows Update
Windows Update Medic Service
Windows 라이선스 관리자 서비스
Windows 모바일 핫스팟 서비스
Windows 백업
Windows 참가자 서비스
Windows 카메라 프레임 서버
Windows 푸시 알림 시스템 서비스
WinHTTP Web Proxy Auto-Discovery Service
Wired AutoConfig
WLAN AutoConfig
WMI Performance Adapter
Workstation
WWAN AutoConfig
Xbox Accessory Management Service
Xbox Game Monitoring
Xbox Live 게임 저장
Xbox Live 네트워킹 서비스
Xbox Live 인증 관리자
결제 및 NFC/SE 관리자
공간 데이터 서비스
기본 인증
데이터 사용량
로컬 프로필 관리자 서비스
무선 송수신 장치 관리 서비스
볼륨 오디오 컴포지터 서비스
소매 데모 서비스
언어 환경 서비스
연결된 디바이스 플랫폼 사용자 서비스_19e1a
연결된 디바이스 플랫폼 서비스
웹 계정 관리자
자녀 보호
자동 표준 시간대 업데이트 프로그램
장치 관리 등록 서비스
포함된 모드
호스트 동기화_19e1a

## 확인되지 않음
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Input\Settings\ControllerProcessor\CursorMagnetism]  
"AttractionRectInsetInDIPS"=dword:00000005  
"DistanceThresholdInDIPS"=dword:00000028  
"MagnetismDelayInMilliseconds"=dword:00000032  
"MagnetismUpdateIntervalInMilliseconds"=dword:00000010  
"VelocityInDIPSPerSecond"=dword:00000168

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Input\Settings\ControllerProcessor\CursorSpeed]  
"CursorUpdateInterval"=dword:00000001  

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Psched]  
"NonBestEffortLimit"=dword:00000000  

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\ServiceProvider]  
"LocalPriority"=dword:00000004  
"HostsPriority"=dword:00000005  
"DnsPriority"=dword:00000006  
"NetbtPriority"=dword:00000007

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Winsock]  
"UseDelayedAcceptance"=dword:00000000  
"MaxSockAddrLength"=dword:00000010  
"MinSockAddrLength"=dword:00000010

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters]  
"EnableICMPRedirect"=dword:00000001  
"EnablePMTUDiscovery"=dword:00000001  
"Tcp1323Opts"=dword:00000000  
"GlobalMaxTcpWindowSize"=dword:000016d0  
"TcpWindowSize"=dword:000016d0  
"MaxConnectionsPerServer"=dword:00000000  
"MaxUserPort"=dword:0000fffe  
"TcpTimedWaitDelay"=dword:00000020  
"EnablePMTUBHDetect"=dword:00000000  
"DisableTaskOffload"=dword:00000000  
"DefaultTTL"=dword:00000040  
"SackOpts"=dword:00000000  
"TcpMaxDupAcks"=dword:00000002

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\{Your NIC's GUID}]  
"TcpAckFrequency"=dword:00000001  
"TcpDelAckTicks"=dword:00000000  
"TCPNoDelay"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSMQ\Parameters]  
"TCPNoDelay"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile]  
"AlwaysOn"=dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management]
"LargeSystemCache"= 0
"IoPageLockLimit"= 2147483648

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\GameDVR]  
"AllowGameDVR"=dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem]  
"NtfsMemoryUsage"=dword:00000000

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System]
"EnableLUA"=dword:00000000

[HKEY_CURRENT_USER\System\GameConfigStore]
"GameDVR_FSEBehaviorMode"=dword:00000002

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers]
~ DISABLEDXMAXIMIZEDWINDOWEDMODE HIGHDPIAWARE DISABLEDWM

[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Taskband]
"NumThumbnails"=dword:00000000
