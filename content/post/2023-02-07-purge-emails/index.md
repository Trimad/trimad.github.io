---
author: Tristan Madden
categories: [ExchangeOnlineManagement]
date: 2023-02-07
lastmod: 2023-04-13
tags: [purge, hard delete, M365]
title: Purge emails with ExchangeOnlineManagement
summary: This PowerShell script imports the Exchange Online Management module and connects to Exchange Online, performs a "hard delete" of phishing emails by using a compliance search action, and then disconnects from Exchange Online PowerShell without a confirmation prompt or any notification text.
usePageBundles: true
thumbnail: "thumbnail.png"
toc: true
---

## Tenant Allow/Block List
Block external email addresses or domains to prevent sending or receiving emails, to or from users in your organization.
<a href="https://security.microsoft.com/tenantAllowBlockList">https://security.microsoft.com/tenantAllowBlockList</a>

## Content Search
Define Content Search criteria that contain only the emails that you would like to purge from your organization.
<a href="https://compliance.microsoft.com/contentsearchv2?viewid=search">https://compliance.microsoft.com/contentsearchv2?viewid=search</a>

## Purge

### Connect to ExchangeOnlineManagement

_In an admin elevated PowerShell session_
```PowerShell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
Install-Module -Name ExchangeOnlineManagement # If not installed already
Import-Module ExchangeOnlineManagement
Connect-IPPSSession # This will launch a Modern Authentication Window
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
