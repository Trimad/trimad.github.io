---
title: Dump WiFi Passwords
author: Tristan Madden
categories: [PowerShell, Security]
tags: [netsh, passwords, reports]
date: 2022-04-11
---

This PowerShell script dumps the saved passwords of all wireless networks saved on a Windows 10 or Windows 11 computer. It accomplishes this by first calling:

```console
netsh wlan show profile
```
and then:
```console
netsh wlan show profile [SSID] key=clear
```
The outputs of these commands are captured in arrays and everything beyond that is just string matching and filtering. The output is saved as "output.csv" to your working directory and then automatically opens output.csv using the <a href="https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/invoke-item?view=powershell-7.2">Invoke-Item</a> cmdlet. 

<script src="https://gist.github.com/Trimad/1829b942568540b704b9ec21cfe99279.js"></script>