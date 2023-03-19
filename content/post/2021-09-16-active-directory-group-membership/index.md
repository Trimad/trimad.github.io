---
title: Active Directory Group Membership
author: Tristan Madden
categories: [PowerShell, Active Directory]
tags: [active directory, domain, reports]
date: 2021-09-16
---

This PowerShell script generates a report showing all groups and group members when run on an on-premises domain controller. The script requires that you import the <a href="https://docs.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps">ActiveDirectory</a> module. The report is in .csv format so the rows may need some manipulation to see all the members if you're opening it in Excel. 

<script src="https://gist.github.com/Trimad/6d9c3037e6c939927f56616a1aa069f0.js"></script>