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

### 사용안함
