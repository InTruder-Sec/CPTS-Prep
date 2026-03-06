---
title: Windows File System
slug: windows-file-system
excerpt: NTFS permissions, file system enumeration, and share access
hidden: false
---

# File System

## Windows File Systems

Windows file systems include: `FAT12, FAT16, FAT32, NTFS, and exFAT`

## NTFS Permissions

| **Permission Type** | **Description** |
| --- | --- |
| Full Control | Allows reading, writing, changing, deleting of files/folders. |
| Modify | Allows reading, writing, and deleting of files/folders. |
| List Folder Contents | Allows for viewing and listing folders and subfolders as well as executing files. Folders only inherit this permission. |
| Read and Execute | Allows for viewing and listing files and subfolders as well as executing files. Files and folders inherit this permission. |
| Write | Allows for adding files to folders and subfolders and writing to a file. |
| Read | Allows for viewing and listing of folders and subfolders and viewing a file's contents. |
| Traverse Folder | This allows or denies the ability to move through folders to reach other files or folders. For example, a user may not have permission to list the directory contents or view files in the documents or web apps directory but with Traverse Folder permissions applied, they can access the backup archive. |

## NTFS File Permissions - icacls

**Integrity Control Access Control List (icacls)** - Tool for managing NTFS permissions

### View Permissions

```powershell
icacls c:\windows

c:\windows NT SERVICE\TrustedInstaller:(F)
           NT SERVICE\TrustedInstaller:(CI)(IO)(F)
           NT AUTHORITY\SYSTEM:(M)
           NT AUTHORITY\SYSTEM:(OI)(CI)(IO)(F)
           BUILTIN\Administrators:(M)
           BUILTIN\Administrators:(OI)(CI)(IO)(F)
           BUILTIN\Users:(RX)
           BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
           CREATOR OWNER:(OI)(CI)(IO)(F)
           APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(RX)
           APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(OI)(CI)(IO)(GR,GE)
           APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(RX)
           APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
```

### Inheritance Flags

- `(CI)`: container inherit
- `(OI)`: object inherit
- `(IO)`: inherit only
- `(NP)`: do not propagate inherit
- `(I)`: permission inherited from parent container

### Basic Access Permissions

- `F` : full access
- `D` : delete access
- `N` : no access
- `M` : modify access
- `RX` : read and execute access
- `R` : read-only access
- `W` : write-only access

### Grant Permissions

Add permissions to the directory for a user:

```powershell
icacls c:\users /grant joe:f
processed file: c:\users
Successfully processed 1 files; Failed processing 0 files
```

### Remove Permissions

Use `/remove` to remove the permissions.

## Network Shares

### View Shared Folders

```bash
net share
```

The `net share` command allows us to view all the shared folders on the system.

### Computer Management

`Computer Management` is another tool we can use to identify and monitor shared resources on a Windows system.

### Viewing Share Access Logs

Share access logs can be viewed in `Event Viewer`.
