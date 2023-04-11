---
author: Tristan Madden
categories: [Shell]
date: 2023-02-22
lastmod: 2023-04-11
tags: [net]
title: net user
thumbnail: "thumbnail.png"
summary: The "net user" command is a Command Prompt (Shell) command used to manage user accounts on a Windows operating system. It can be used to create, modify, or delete user accounts, as well as to change passwords and manage group memberships.
usePageBundles: true
toc: true
---

The "net user" command is a Command Prompt (Shell) command used to manage user accounts on a Windows operating system. It can be used to create, modify, or delete user accounts, as well as to change passwords and manage group memberships.

## Add a user

```Shell
net user <username> <password> /add
```

## Add user to group

```Shell
net localgroup <groupname> <domain>\<username> /add
```

## Check if account is locked

_The find is case sensitive:_

```Shell
net user /domain <username> | find "Account active"
```

## Reset password
_Domain_
```Shell
net user <username> <password> /domain /active:Yes
```
_Local_
```Shell
net user <username> <password>
```

## Unlock account

```Shell
Net user <username> /domain /active:yes
```