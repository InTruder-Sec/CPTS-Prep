# File System

- Windows file systems: `FAT12, FAT16, FAT32, NTFS, and exFAT`

| **Permission Type** | **Description** |
| --- | --- |
| Full Control | Allows reading, writing, changing, deleting of files/folders. |
| Modify | Allows reading, writing, and deleting of files/folders. |
| List Folder Contents | Allows for viewing and listing folders and subfolders as well as executing files. Folders only inherit this permission. |
| Read and Execute | Allows for viewing and listing files and subfolders as well as executing files. Files and folders inherit this permission. |
| Write | Allows for adding files to folders and subfolders and writing to a file. |
| Read | Allows for viewing and listing of folders and subfolders and viewing a file's contents. |
| Traverse Folder | This allows or denies the ability to move through folders to reach other files or folders. For example, a user may not have permission to list the directory contents or view files in the documents or web apps directory in this example c:\users\bsmith\documents\webapps\backups\backup_02042020.zip but with Traverse Folder permissions applied, they can access the backup archive. |
- NTFS file permissions - **Integrity Control Access Control List (icacls)**

```powershell
C:\htb> icacls c:\windows
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

- `(CI)`: container inherit
- `(OI)`: object inherit
- `(IO)`: inherit only
- `(NP)`: do not propagate inherit
- `(I)`: permission inherited from parent container

Basic access permissions are as follows:

- `F` : full access
- `D` : delete access
- `N` : no access
- `M` : modify access
- `RX` : read and execute access
- `R` : read-only access
- `W` : write-only access

Add permissions to the dir for that user

```powershell
icacls c:\users /grant joe:f
processed file: c:\users
Successfully processed 1 files; Failed processing 0 files
```

`/remove` to remove the permissions

`net share` command allows us to view all the shared folders on the system

`Computer Management` is another tool we can use to identify and monitor shared resources on a Windows system.

Viewing Share access logs in Event Viewer `Event Viewer`