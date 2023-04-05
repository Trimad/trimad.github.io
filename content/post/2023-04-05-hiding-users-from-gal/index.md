---
author: Tristan Madden
categories: [Exchange]
date: 2023-04-05
draft: false
summary: "Scripts for hiding users from the Global Address List"
tags: [msExchHideFromAddressLists, ActiveDirectory, ExchangeOnlineManagement]
thumbnail: "thumbnail.png"
title: "Hiding users from GAL"
toc: true
usePageBundles: true
---

## From ActiveDirectory

### Contacts

This PowerShell script does the following:

* Imports the Active Directory module to enable interaction with Active Directory objects.
* Retrieves all contact objects in Active Directory and sorts them alphabetically.
* Loops through each contact, checking if the 'msExchHideFromAddressLists' attribute is set or not.
* Prints a message for each contact, indicating if they are hidden from address lists or if the attribute is not set.

```PowerShell
Import-Module ActiveDirectory

$contacts = Get-ADObject -Filter {objectClass -eq "contact"} | sort

foreach ($contact in $contacts) {
    $contactDN = $contact.DistinguishedName
    $hideFromAddressLists = (Get-ADObject -Identity $contactDN -Properties msExchHideFromAddressLists).msExchHideFromAddressLists
    if ([string]::IsNullOrEmpty($hideFromAddressLists)) {
        Write-Host "Contact $($contact.name) does not have the 'msExchHideFromAddressLists' attribute"
    } else {
        Write-Host "Contact $($contact.name) is hidden from address lists: $($hideFromAddressLists)"
    }
}
```

### Contacts that are members of a particular Distribution List
This PowerShell script performs the following actions:

* It installs and imports the ExchangeOnlineManagement module, which is required to manage Exchange Online.
* It connects to Exchange Online to allow further management.
* It retrieves the distinguished name of a distribution list called "external users" and finds all contacts belonging to this list.
* For each contact found, it updates their properties to hide them from address lists.

```PowerShell
Install-Module -Name ExchangeOnlineManagement # if not installed already
Import-Module ExchangeOnlineManagement
Connect-ExchangeOnline

# Get the distinguished name of the "external users" distribution list
$externalUsersDL = (Get-ADGroup -Filter {name -eq "external users"}).DistinguishedName
 
# Get all contacts that belong to the "external users" distribution list
$contacts = Get-ADObject -Filter {objectClass -eq "contact" -and memberOf -eq $externalUsersDL} | sort
 
foreach ($contact in $contacts) {
    $contactDN = $contact.DistinguishedName
    $hideFromAddressLists = (Get-ADObject -Identity $contactDN -Properties Set-ADObject -Identity $contactDN -Replace @{msExchHideFromAddressLists=$true}
}
```

## From ExchangeOnlineManagement

### Contacts

This PowerShell script performs the following actions:

* It installs and imports the Exchange Online Management module, which is used to manage Microsoft Exchange Online.
* It connects to Exchange Online to access the necessary data and functions.
* It retrieves a list of members from the "External Users" distribution group.
* For each member in the list, it displays their name and the current 'HiddenFromAddressListsEnabled' status, then sets the 'HiddenFromAddressListsEnabled' property to true, hiding the user from address lists.

```PowerShell
Install-Module -Name ExchangeOnlineManagement # if not installed already
Import-Module ExchangeOnlineManagement
Connect-ExchangeOnline
 
$ExternalUsers = Get-DistributionGroupMember -Identity "External Users"
foreach ($user in $ExternalUsers) {
    $mailContact = Get-MailContact -Identity $user.Name | Select-Object Name, HiddenFromAddressListsEnabled
    Write-Host "Name: $($mailContact.Name), HiddenFromAddressListsEnabled: $($mailContact.HiddenFromAddressListsEnabled)"
    
    # Set HiddenFromAddressListsEnabled to True
    Set-MailContact -Identity $user.Name -HiddenFromAddressListsEnabled $true
}
```