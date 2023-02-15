---
author: Tristan Madden
categories: [ExchangeOnlineManagement, O365, M365]
date: 2023-02-07
lastmod: 2023-02-15
tags: [purge, hard delete]
title: Purge emails with ExchangeOnlineManagement
---
<h2>Content Search</h2>
<a href="https://compliance.microsoft.com/contentsearchv2?viewid=search">https://compliance.microsoft.com/contentsearchv2?viewid=search</a>

<h2>Purge</h2>

```PowerShell
# Import the Exchange Online Management module into the current PowerShell sessionand establish a connection to Exchange Online using the specified User Principal Name (UPN)
Import-Module ExchangeOnlineManagement
Connect-IPPSSession -UserPrincipalName admin@thecompany.org
 
# Performed a "hard delete" of phishing emails
New-ComplianceSearchAction -SearchName "name_of_content_search" -Purge -PurgeType HardDelete
 
# Checked status of compliance serach action:
Get-ComplianceSearchAction -Identity "name_of_content_search_Purge" | Format-List

# Silently disconnect from Exchange Online PowerShell or Security & Compliance PowerShell without a confirmation prompt or any notification text.
Disconnect-ExchangeOnline -Confirm:$false
```