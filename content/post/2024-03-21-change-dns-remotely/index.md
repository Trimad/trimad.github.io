---
author: Tristan Madden
categories: [Python]
date: 2024-03-22
#lastmod: 2023-10-17
draft: false
featured: true
summary: "Steps that are useful for unbricking a network with a bad DNS config remotely."
tags: [Python]
thumbnail: "thumbnail.png"
title: "Change DNS Remotely"
toc: true
usePageBundles: true
---

Steps that are useful for unbricking a network with a bad DNS config remotely.

```shell
# From Shell
wmic /node:"HOSTNAME" nicconfig get Description, SettingID, IPEnabled
 
# From Shell
wmic /node:"HOSTNAME" nicconfig where SettingID='{4C2E5961-1B25-4AE9-852C-0A285E891244}' call SetDNSServerSearchOrder ("10.0.0.4")
 
# From Shell
wmic /node:"HOSTNAME" nicconfig where SettingID='{FF2CE891-C0C8-48A4-8A97-286559FD1AF0}' call SetDNSServerSearchOrder ("10.0.0.4")
 
# From PowerShell (this checks to see what the DNS server is actually set to on the remote PC)
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -ComputerName HOSTNAME -Filter "IPEnabled = True" | Select-Object -Property Description
```