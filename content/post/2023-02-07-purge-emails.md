---
author: Tristan Madden
categories: [ExchangeOnlineManagement, O365, M365]
date: 2023-02-07
tags: [purge, hard delete]
title: Purge emails with ExchangeOnlineManagement
---
<h1>Content Search</h1>
<a href="https://compliance.microsoft.com/contentsearchv2?viewid=search">https://compliance.microsoft.com/contentsearchv2?viewid=search</a>

<h1>Purge</h1>
```PowerShell
#Connected to Security & Compliance PowerShell
Import-Module ExchangeOnlineManagement
Connect-IPPSSession -UserPrincipalName admin@thecompany.org
 
# Performed a "hard delete" of phishing emails
New-ComplianceSearchAction -SearchName "name_of_content_search" -Purge -PurgeType HardDelete
 
# Checked status of compliance serach action:
Get-ComplianceSearchAction -Identity "name_of_content_search_Purge" | Format-List
```