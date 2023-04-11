---
author: Tristan Madden
categories: [ExchangeOnlineManagement]
date: 2023-02-07
lastmod: 2023-04-10
tags: [purge, hard delete, M365]
title: Purge emails with ExchangeOnlineManagement
summary: This PowerShell script imports the Exchange Online Management module and connects to Exchange Online, performs a "hard delete" of phishing emails by using a compliance search action, and then disconnects from Exchange Online PowerShell without a confirmation prompt or any notification text.
usePageBundles: true
thumbnail: "thumbnail.png"
toc: true
---

## Content Search

<a href="https://compliance.microsoft.com/contentsearchv2?viewid=search">https://compliance.microsoft.com/contentsearchv2?viewid=search</a>

## Purge

### Connect to ExchangeOnlineManagement

```PowerShell
Install-Module -Name ExchangeOnlineManagement # if not installed already
Import-Module ExchangeOnlineManagement
Connect-IPPSSession
```

### Confirm that your content search is valid

```PowerShell
Get-ComplianceSearch | Sort-Object JobEndTime # if you don't see your content search on this list something is wrong
```

### Perform a "hard delete" of the content search


{{% notice warning "WARNING" %}}
This cannot be undone!
{{% /notice %}}

```PowerShell
New-ComplianceSearchAction -SearchName "name_of_content_search" -Purge -PurgeType HardDelete
 ```

### Check the status of hard delete
For the -Identity, just take the name of the content search and append ```_Purge``` to it. 
```PowerShell
Get-ComplianceSearchAction -Identity "name_of_content_search_Purge" | Format-List
```

### Disconnect from ExchangeOnlineManagement
```PowerShell
Disconnect-ExchangeOnline
```
