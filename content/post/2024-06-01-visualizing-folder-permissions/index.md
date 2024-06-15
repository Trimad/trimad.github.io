---
author: Tristan Madden
categories: [Visualization]
date: 2024-06-01
draft: false
featured: false
summary: "Useful scripts for visualuzing folder perimssions."
tags: [permissions, security]
thumbnail: "thumbnail.png"
title: "Visualizing Folder Permissions"
toc: true
usePageBundles: true
---

This blog post provides a comprehensive guide to exporting folder permissions to a JSON file using PowerShell and visualizing these permissions using Python. The process involves two main scripts:

## Exporting Folder Permissions with PowerShell

This script retrieves and exports folder permissions from a specified directory and its subdirectories. It uses the Get-Acl cmdlet to gather access control information and stores this data in a custom PowerShell object. The collected permissions are then converted to JSON format and saved to a file.

```PowerShell
# Define the folder path and output file path as variables
$FolderPath = "C:\Users\WDAGUtilityAccount"
$OutputFile = "C:\Users\WDAGUtilityAccount\FolderPermissions.json"

function Get-FolderPermissions {
    param (
        [string]$Path
    )
    
    if (-Not (Test-Path -Path $Path)) {
        Write-Warning "Path '$Path' does not exist."
        return $null
    }

    $acl = Get-Acl -Path $Path
    $permissions = @()

    foreach ($access in $acl.Access) {
        $permissions += [PSCustomObject]@{
            AccessControlType = [int]$access.AccessControlType
            BitwisePermissions = [int]$access.FileSystemRights
            ChangePermissions = [int]($access.FileSystemRights -band [System.Security.AccessControl.FileSystemRights]::ChangePermissions)
            CreateFiles_WriteData = [int]($access.FileSystemRights -band [System.Security.AccessControl.FileSystemRights]::WriteData)
            CreateFolders_AppendData = [int]($access.FileSystemRights -band [System.Security.AccessControl.FileSystemRights]::AppendData)
            Delete = [int]($access.FileSystemRights -band [System.Security.AccessControl.FileSystemRights]::Delete)
            DeleteSubfoldersAndFiles = [int]($access.FileSystemRights -band [System.Security.AccessControl.FileSystemRights]::DeleteSubdirectoriesAndFiles)
            FullControl = [int]($access.FileSystemRights -band [System.Security.AccessControl.FileSystemRights]::FullControl)
            IdentityReference = $access.IdentityReference
            InheritanceFlags = [int]$access.InheritanceFlags
            ListFolder_ReadData = [int]($access.FileSystemRights -band [System.Security.AccessControl.FileSystemRights]::ListDirectory)
            PropagationFlags = [int]$access.PropagationFlags
            ReadAttributes = [int]($access.FileSystemRights -band [System.Security.AccessControl.FileSystemRights]::ReadAttributes)
            ReadExtendedAttributes = [int]($access.FileSystemRights -band [System.Security.AccessControl.FileSystemRights]::ReadExtendedAttributes)
            ReadPermissions = [int]($access.FileSystemRights -band [System.Security.AccessControl.FileSystemRights]::ReadPermissions)
            TakeOwnership = [int]($access.FileSystemRights -band [System.Security.AccessControl.FileSystemRights]::TakeOwnership)
            TraverseFolder_ExecuteFile = [int]($access.FileSystemRights -band [System.Security.AccessControl.FileSystemRights]::ExecuteFile)
            WriteAttributes = [int]($access.FileSystemRights -band [System.Security.AccessControl.FileSystemRights]::WriteAttributes)
            WriteExtendedAttributes = [int]($access.FileSystemRights -band [System.Security.AccessControl.FileSystemRights]::WriteExtendedAttributes)
        }
    }

    return [PSCustomObject]@{
        Path = $Path
        Permissions = $permissions
    }
}

function Get-AllFolderPermissions {
    param (
        [string]$RootPath
    )
    
    $result = @()
    $folders = Get-ChildItem -Path $RootPath -Recurse -Directory
    
    foreach ($folder in $folders) {
        $folderPermissions = Get-FolderPermissions -Path $folder.FullName
        if ($folderPermissions -ne $null) {
            $result += $folderPermissions
        }
    }

    $rootPermissions = Get-FolderPermissions -Path $RootPath
    if ($rootPermissions -ne $null) {
        $result += $rootPermissions
    }

    return $result
}

$allPermissions = Get-AllFolderPermissions -RootPath $FolderPath
$json = $allPermissions | ConvertTo-Json -Depth 100

# Save JSON without BOM
$json | Out-File -FilePath $OutputFile -Encoding ascii

Write-Host "Permissions saved to $OutputFile"
```

## Visualizing Folder Permissions with Python

This script reads the JSON file created by the PowerShell script and visualizes the folder permissions using the NetworkX and PyVis libraries. Each unique identity reference from the permissions is processed to generate a directed graph. Nodes represent folder paths, and edges denote the hierarchical structure. The graphs are styled with random colors and saved as interactive HTML files.

```python
import json
import networkx as nx
from pyvis.network import Network
import random
from pathlib import Path

def generate_random_color():
    """Generate a random hex color."""
    return "#{:06x}".format(random.randint(0x000000, 0xFFFFFF))

print("Loading JSON data...")
# Load the JSON data
with open('FolderPermissions.json', 'r') as file:
    data = json.load(file)
print("JSON data loaded.")

# Extract all unique IdentityReference values
identity_references = set()
for item in data:
    for perm in item['Permissions']:
        identity_references.add(perm['IdentityReference']['Value'])

print(f"Found {len(identity_references)} unique identity references.")

# Create a directory to save the visualizations
output_dir = Path("folder_permissions_visualizations")
output_dir.mkdir(exist_ok=True)

# Process each identity reference
for identity in identity_references:
    print(f"Processing identity: {identity}")

    # Create a NetworkX graph for the current identity
    G = nx.DiGraph()

    # Extract the paths for the current identity
    paths = [item['Path'] for item in data if any(perm['IdentityReference']['Value'] == identity for perm in item['Permissions'])]

    print(f"Found {len(paths)} paths for identity {identity}.")

    # Add nodes and edges to the graph
    for path in paths:
        parts = path.split('\\')
        for i in range(len(parts)):
            if i == 0:
                G.add_node(parts[i])
            else:
                G.add_edge('\\'.join(parts[:i]), '\\'.join(parts[:i+1]))

    print(f"Nodes and edges added for identity {identity}.")

    # Create a PyVis network from the NetworkX graph
    net = Network(notebook=False, width='100vw', height='93vh', bgcolor="#1a1a1a", font_color="#cccccc")

    # Add nodes and edges to the PyVis network
    for node in G.nodes:
        color = generate_random_color()
        net.add_node(node, label=node.split('\\')[-1], title=node, color=color)

    for edge in G.edges:
        net.add_edge(edge[0], edge[1])

    # Set hierarchical layout with top-to-bottom direction
    net.set_options("""
    const options = {
      "configure": {
        "enabled": true
      },
      "nodes": {
        "shape": "dot",
        "size": 30
      },
      "edges": {
        "smooth": {
          "enabled": false
        }
      },
      "layout": {
        "hierarchical": {
          "enabled": true,
          "edgeMinimization": true,
          "levelSeparation": 150,
          "nodeSpacing": 300,
          "treeSpacing": 150,
          "direction": "UD",
          "sortMethod": "directed"
        }
      },
      "interaction": {
        "hover": true
      },
      "physics": {
        "enabled": false
      }
    }
    """)

    # Save the network for the current identity
    output_file = output_dir / f"{identity.replace('\\', '_')}_permissions.html"
    print(f"Saving network visualization to {output_file}...")
    net.write_html(str(output_file), notebook=False)
    print(f"Network visualization saved for identity {identity}.")

print("All visualizations saved.")

```

This approach provides a clear and structured method for administrators to audit and visualize folder permissions in their environment.

This came in handy for visualizing the bitwise mask:

```powershell
[System.Enum]::GetValues([System.Security.AccessControl.FileSystemRights]) | ForEach-Object {
    [PSCustomObject]@{
        Name = $_
        Value = [int]$_
    }
} | Format-Table -AutoSize
```

The result is:

```
                    ReadData       1
                    ReadData       1
                 CreateFiles       2
                 CreateFiles       2
                  AppendData       4
                  AppendData       4
      ReadExtendedAttributes       8
     WriteExtendedAttributes      16
                 ExecuteFile      32
                 ExecuteFile      32
DeleteSubdirectoriesAndFiles      64
              ReadAttributes     128
             WriteAttributes     256
                       Write     278
                      Delete   65536
             ReadPermissions  131072
                        Read  131209
              ReadAndExecute  131241
                      Modify  197055
           ChangePermissions  262144
               TakeOwnership  524288
                 Synchronize 1048576
                 FullControl 2032127
```