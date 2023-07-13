---
author: Tristan Madden
categories: [shell,powershell]
date: 2023-06-24
featured: false
summary: "One-liners that can join a workstation to AD with Shell and PowerShell."
tags: [ai, images, video]
thumbnail: "thumbnail.png"
title: Join workstation to Active Directory
toc: true
usePageBundles: true
---

## Shell

```shell
powershell -Command "& {$password = ConvertTo-SecureString 'thepassword' -AsPlainText -Force; $credential = New-Object System.Management.Automation.PSCredential ('domain\username', $password); Add-Computer -DomainName 'thedomainname' -Credential $credential -Restart -Force}"
```
## PowerShell
```PowerShell
& {$password = ConvertTo-SecureString 'thepassword' -AsPlainText -Force; $credential = New-Object System.Management.Automation.PSCredential ('domain\username', $password); Add-Computer -DomainName 'thedomainname' -Credential $credential -Restart -Force}
```
