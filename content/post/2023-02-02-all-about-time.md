---
title: All About Time
author: Tristan Madden
categories: [Shell, PowerShell]
tags: []
date: 2023-02-02
lastmod: 2023-03-14
toc: true
---

## wmic

### LastBootUpTime

```Shell
wmic path Win32_OperatingSystem get LastBootUpTime
```

## systeminfo

### System Boot Time
```Shell
systeminfo | find "System Boot Time"
```

## Win32_OperatingSystem

### LastBootUpTime

```PowerShell
(get-date) - (gcim Win32_OperatingSystem).LastBootUpTime
```

```Shell
powershell.exe -c "(get-date) - (gcim Win32_OperatingSystem).LastBootUpTime"
```

## TIME
Get the current system time.
```Shell
TIME /T
```
## tzutil

### Change time zone
```Shell
tzutil /s "Eastern Standard Time"
```
