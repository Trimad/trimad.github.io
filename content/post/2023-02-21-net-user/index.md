---
author: Tristan Madden
categories: [Shell]
date: 2023-02-22
tags: [net]
title: net user
thumbnail: "thumbnail.png"
summary: The "net user" command is a Command Prompt (Shell) command used to manage user accounts on a Windows operating system. It can be used to create, modify, or delete user accounts, as well as to change passwords and manage group memberships.
usePageBundles: true
---

The "net user" command is a Command Prompt (Shell) command used to manage user accounts on a Windows operating system. It can be used to create, modify, or delete user accounts, as well as to change passwords and manage group memberships.

<h3>Check if account is locked</h3>

_The find is case sensitive:_

```Shell
net user /domain <username> | find "Account active"
```

<h3>Unlock account</h3>

```Shell
Net user <username> /domain /active:yes
```

<h3>Reset password</h3>

```Shell
net user <username> <password> /domain /active:Yes
```

<h3>Add to group</h3>

```Shell
net localgroup <groupname> <domain>/<username> /add
```

