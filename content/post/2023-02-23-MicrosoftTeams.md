---
author: Tristan Madden
categories: [PowerShell]
date: 2023-02-23
tags: [microsoft teams]
title: MicrosoftTeams PowerShell module
summary: PowerShell scripts for Microsoft Teams stuff.
---

PowerShell scripts for Microsoft Teams stuff.

<h3>Get all owners of all teams and team channels</h3>

```PowerShell
# Documentation: https://learn.microsoft.com/en-us/powershell/module/teams/?view=teams-ps

# Run the following command to install the latest PowerShellGet:
Install-Module -Name PowerShellGet -Force -AllowClobber

# Install the Teams PowerShell Module.
Install-Module -Name MicrosoftTeams -Force -AllowClobber

# To start working with Microsoft Teams PowerShell module, sign in with your Azure credentials.
Connect-MicrosoftTeams

$teams = Get-Team | Select-Object DisplayName,GroupId

$teamOwners=@() 

$teamChannelOwners=@() 

$teamOwners = foreach ($team in $teams) {

    $groupId = $team.GroupId
    
    $teamDisplayName = $team.DisplayName

    Get-TeamUser -GroupId $groupId -Role Owner | Select-Object *,@{Name="TeamName";Expression={$teamDisplayName}},@{Name="GroupId";Expression={$groupId}}

    $channels = Get-TeamAllChannel -GroupId $groupId

    $teamChannelOwners += foreach ($channel in $channels) {
    
        $channelDisplayName = $channel.DisplayName
    
        Get-TeamChannelUser -GroupId $groupId -DisplayName $channelDisplayName -Role Owner | Select-Object *,@{Name="ChannelName";Expression={$channelDisplayName}},@{Name="GroupId";Expression={$groupId}}
    
    }
}

$teamOwners | Export-Csv -Path "team-owners.csv" -NoTypeInformation

$teamChannelOwners | Export-Csv -Path "team-channel-owners.csv" -NoTypeInformation
```