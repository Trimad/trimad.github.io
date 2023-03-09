---
title: All About Time
author: Tristan Madden
categories: [CMD, PowerShell]
tags: [time, windows]
date: 2023-02-02
---

The first command, "wmic path Win32_OperatingSystem get LastBootUpTime", is a Windows Management Instrumentation (WMI) command that retrieves the time the operating system was last booted. The second command, "systeminfo | find "System Boot Time"", is a Windows Command Prompt command that uses the systeminfo command to retrieve system information, which is then piped (|) to the find command to search for the specific line containing "System Boot Time". The third command, "(get-date) - (gcim Win32_OperatingSystem).LastBootUpTime", is a PowerShell command that calculates the elapsed time since the last system boot by subtracting the value of LastBootUpTime retrieved from the WMI class "Win32_OperatingSystem" using the Get-CIMInstance (gcim) cmdlet from the current date obtained using the Get-Date cmdlet.

<h3>wmic method</h3>

```Shell
wmic path Win32_OperatingSystem get LastBootUpTime
```
<h3>systeminfo method</h3>

```Shell
systeminfo | find "System Boot Time"
```

<h3>Win32_OperatingSystem method</h3>

```PowerShell
(get-date) - (gcim Win32_OperatingSystem).LastBootUpTime
```

```Shell
powershell.exe -c "(get-date) - (gcim Win32_OperatingSystem).LastBootUpTime"
```
