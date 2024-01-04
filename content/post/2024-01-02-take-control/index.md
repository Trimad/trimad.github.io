---
author: Tristan Madden
categories: [PowerShell]
date: 2024-02-12
#lastmod: 2023-10-17
draft: true
featured: true
summary: "Everything and aynthing about Take Control."
tags: [PowerShell]
thumbnail: "thumbnail.png"
title: "Take Control"
toc: true
usePageBundles: true
---

```powershell
# Get a list of all services:
Invoke-Command -ComputerName SOLARWINDS -ScriptBlock { Get-Service } -Credential (Get-Credential) | Export-Csv -Path 'C:\output.csv' -NoTypeInformation
 
# Restart BASupportExpressStandaloneService_N_Central
Invoke-Command -ComputerName SOLARWINDS -ScriptBlock { Restart-Service -Name 'BASupportExpressStandaloneService_N_Central' } -Credential (Get-Credential)
 
# Restart BASupportExpressSrvcUpdater_N_Central
Invoke-Command -ComputerName SOLARWINDS -ScriptBlock { Restart-Service -Name 'BASupportExpressSrvcUpdater_N_Central' } -Credential (Get-Credential)
```