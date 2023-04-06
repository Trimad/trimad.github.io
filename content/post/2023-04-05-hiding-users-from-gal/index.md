---
author: Tristan Madden
categories: [Exchange]
date: 2023-04-05
lastmod: 2023-04-06
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

This PowerShell script performs the following actions:

* It installs and imports the ExchangeOnlineManagement module, which is required to manage Exchange Online.
* It connects to Exchange Online to allow further management.
* It retrieves the distinguished name of a distribution list called "external users" and finds all contacts belonging to this list.
* For each contact found, it updates their properties to hide them from address lists.

```PowerShell
Import-Module ActiveDirectory

# Get the distinguished name of the "external users" distribution list
$externalUsersDL = (Get-ADGroup -Filter {name -eq "external users"}).DistinguishedName
 
# Get all contacts that belong to the "external users" distribution list
$contacts = Get-ADObject -Filter {objectClass -eq "contact" -and memberOf -eq $externalUsersDL} | sort
 
foreach ($contact in $contacts) {
    $contactDN = $contact.DistinguishedName
    $hideFromAddressLists = (Get-ADObject -Identity $contactDN -Properties Set-ADObject -Identity $contactDN -Replace @{msExchHideFromAddressLists=$true}
}
```

### Users

This PowerShell script performs the following actions:

* Imports the Active Directory module, which enables the script to interact with AD objects like users and groups.
* Retrieves a list of users from the "DisabledUsers" organizational unit (OU) in the Active Directory, including their 'msExchHideFromAddressLists' property, and sorts them.
* Iterates through each user in the list and checks the value of their 'msExchHideFromAddressLists' property.
* Depending on the property value, the script either sets or updates the 'msExchHideFromAddressLists' attribute to 'True' and outputs a corresponding message; if it's already set to 'True', the script simply outputs a message indicating this.

```PowerShell
Import-Module ActiveDirectory
 
$disabledUsersOU = "OU=DisabledUsers,OU=AFO Sites,DC=afo,DC=com"
$users = Get-ADUser -SearchBase $disabledUsersOU -Filter * -Properties msExchHideFromAddressLists | sort
 
foreach ($user in $users) {
    $userDN = $user.DistinguishedName
    $hideFromAddressLists = $user.msExchHideFromAddressLists
    if ([string]::IsNullOrEmpty($hideFromAddressLists)) {
        Set-ADUser -Identity $userDN -Add @{msExchHideFromAddressLists=$true}
        Write-Host "User $($user.name) did not have the 'msExchHideFromAddressLists' attribute and it has been set to 'True'"
    } elseif (!$hideFromAddressLists) {
        Set-ADUser -Identity $userDN -Replace @{msExchHideFromAddressLists=$true}
        Write-Host "User $($user.name) had the 'msExchHideFromAddressLists' attribute set to 'False' and it has been set to 'True'"
    } else {
        Write-Host "User $($user.name) already has the 'msExchHideFromAddressLists' attribute set to 'True'"
    }
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