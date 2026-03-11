---
title: SMB/CIFS Services
excerpt: Server Message Block enumeration and exploitation techniques
hidden: false
---

# SMB

`Server Message Block` (`SMB`) is a client-server protocol that regulates access to files and entire directories and other network resources such as printers, routers, or interfaces released for the network. Information exchange between different system processes can also be handled based on the SMB protocol.

## Samba

As mentioned earlier, there is an alternative implementation of the SMB server called Samba, which is developed for Unix-based operating systems. Samba implements the Common Internet File System (`CIFS`) network protocol. [CIFS](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/934c2faa-54af-4526-ac74-6a24d126724e) is a dialect of SMB, meaning it is a specific implementation of the SMB protocol originally created by Microsoft. This allows Samba to communicate effectively with newer Windows systems.

However, `CIFS` is considered a specific version of the SMB protocol, primarily aligning with `SMB version 1`. When SMB commands are transmitted over Samba to an older NetBIOS service, connections typically occur over TCP ports `137`, `138`, and `139`. In contrast, CIFS operates over TCP port `445` exclusively.

## SMB Versions

| **SMB Version** | **Supported** | **Features** |
| --- | --- | --- |
| CIFS | Windows NT 4.0 | Communication via NetBIOS interface |
| SMB 1.0 | Windows 2000 | Direct connection via TCP |
| SMB 2.0 | Windows Vista, Windows Server 2008 | Performance upgrades, improved message signing, caching feature |
| SMB 2.1 | Windows 7, Windows Server 2008 R2 | Locking mechanisms |
| SMB 3.0 | Windows 8, Windows Server 2012 | Multichannel connections, end-to-end encryption, remote storage access |
| SMB 3.0.2 | Windows 8.1, Windows Server 2012 R2 |  |
| SMB 3.1.1 | Windows 10, Windows Server 2016 | Integrity checking, AES-128 encryption |

## NetBIOS and Workgroups

We know that Samba is suitable for both Linux and Windows systems. In a network, each host participates in the same `workgroup`. A workgroup is a group name that identifies an arbitrary collection of computers and their resources on an SMB network. There can be multiple workgroups on the network at any given time.

IBM developed an `application programming interface` (`API`) for networking computers called the `Network Basic Input/Output System` (`NetBIOS`). The NetBIOS API provided a blueprint for an application to connect and share data with other computers. In a NetBIOS environment, when a machine goes online, it needs a name, which is done through the so-called `name registration` procedure. Either each host reserves its hostname on the network, or the [NetBIOS Name Server](https://networkencyclopedia.com/netbios-name-server-nbns/) (`NBNS`) is used for this purpose. It also has been enhanced to [Windows Internet Name Service](https://networkencyclopedia.com/windows-internet-name-service-wins/) (`WINS`).

## Samba Configuration

### Default Configuration

```bash
cat /etc/samba/smb.conf | grep -v "#\|\;"
```

### Configuration Settings

| **Setting** | **Description** |
| --- | --- |
| `[sharename]` | The name of the network share. |
| `workgroup = WORKGROUP/DOMAIN` | Workgroup that will appear when clients query. |
| `path = /path/here/` | The directory to which user is to be given access. |
| `server string = STRING` | The string that will show up when a connection is initiated. |
| `unix password sync = yes` | Synchronize the UNIX password with the SMB password? |
| `usershare allow guests = yes` | Allow non-authenticated users to access defined share? |
| `map to guest = bad user` | What to do when a user login request doesn't match a valid UNIX user? |
| `browseable = yes` | Should this share be shown in the list of available shares? |
| `guest ok = yes` | Allow connecting to the service without using a password? |
| `read only = yes` | Allow users to read files only? |
| `create mask = 0700` | What permissions need to be set for newly created files? |

### Dangerous Settings

| **Setting** | **Description** |
| --- | --- |
| `logon script = script.sh` | What script needs to be executed on the user's login? |
| `magic script = script.sh` | Which script should be executed when the script gets closed? |
| `magic output = script.out` | Where the output of the magic script needs to be stored? |

## SMB Enumeration

### Check Connections with smbstatus

From the administrative point of view, we can check these connections using `smbstatus`. Apart from the Samba version, we can also see who, from which host, and which share the client is connected.

### RPCclient Enumeration

One of the handy tools for this is `rpcclient`. This is a tool to perform MS-RPC functions.

The [Remote Procedure Call](https://www.geeksforgeeks.org/remote-procedure-call-rpc-in-operating-system/) (`RPC`) is a concept and, therefore, also a central tool to realize operational and work-sharing structures in networks and client-server architectures. The communication process via RPC includes passing parameters and the return of a function value.

```bash
rpcclient -U"" 10.129.14.128
Enter WORKGROUP\'s password:
rpcclient $>
```

The `rpcclient` offers us many different requests with which we can execute specific functions on the SMB server to get information. A complete list of all these functions can be found on the [man page](https://www.samba.org/samba/docs/current/man-html/rpcclient.1.html) of the rpcclient.

### RPCclient Queries

| **Query** | **Description** |
| --- | --- |
| `srvinfo` | Server information. |
| `enumdomains` | Enumerate all domains that are deployed in the network. |
| `querydominfo` | Provides domain, server, and user information of deployed domains. |
| `netshareenumall` | Enumerates all available shares. |
| `netsharegetinfo <share>` | Provides information about a specific share. |
| `enumdomusers` | Enumerates all domain users. |
| `queryuser <RID>` | Provides information about a specific user. |
