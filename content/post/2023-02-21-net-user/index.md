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
net user USERNAME PASSWORD /add
```

## Add user to group

```Shell
net localgroup GROUPNAME DOMAIN\USERNAME /add
```

## Check if account is locked

_The find is case sensitive:_

```Shell
net user /domain USERNAME | find "Account active"
```

## Reset password
_Domain_
```Shell
net user USERNAME PASSWORD /domain /active:Yes
```
_Local_
```Shell
net user USERNAME PASSWORD
```

## Unlock account

```Shell
net user USERNAME /domain /active:yes
```

## Lock an account

```Shell
net user USERNAME PASSWORD /domain /active:no
```