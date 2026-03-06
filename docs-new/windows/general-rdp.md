---
title: Windows General & RDP
excerpt: Windows information gathering and Remote Desktop Protocol usage
hidden: false
---

# General & RDP

## Getting Windows Information

```powershell
Get-WmiObject -Class win32_OperationSystem | select Version, BuildNumber
```

This gets information on the OS where `win32_OperationSystem` is a class instance.

### WMI Classes

- `Win32_Process` - Get a process listing
- `Win32_Service` - Get a listing of services
- `Win32_Bios` - Get [Basic Input/Output System](https://en.wikipedia.org/wiki/BIOS) (`BIOS`) information

We can use the `ComputerName` parameter to get information about remote computers. `Get-WmiObject` can be used to start and stop services on local and remote computers, and more.

## Remote Desktop Protocol (RDP)

### RDP Connection Logs

Logs/saved profiles Remote Desktop Files (`.rdp`)

### xfreerdp

**xfreerdp** - Linux tool to use RDP to connect to Windows systems

**Alternatives:** [Remmina](https://remmina.org/) and [rdesktop](http://www.rdesktop.org/)

```bash
xfreerdp /v:<targetIp> /u:htb-student /p:Password
```

## Windows Services & Processes

Windows services are managed via the Service Control Manager (SCM) system, accessible via the `services.msc` MMC add-in.

### Get Running Services

```powershell
Get-Service | ? {$_.Status -eq "Running"} | select -First 2 | fl
```

### Critical Windows Processes

| **Service** | **Description** |
| --- | --- |
| smss.exe | Session Manager SubSystem. Responsible for handling sessions on the system. |
| csrss.exe | Client Server Runtime Process. The user-mode portion of the Windows subsystem. |
| wininit.exe | Starts the Wininit file .ini file that lists all of the changes to be made to Windows when the computer is restarted after installing a program. |
| logonui.exe | Used for facilitating user login into a PC |
| lsass.exe | The Local Security Authentication Server verifies the validity of user logons to a PC or server. It generates the process responsible for authenticating users for the Winlogon service. |
| services.exe | Manages the operation of starting and stopping services. |
| winlogon.exe | Responsible for handling the secure attention sequence, loading a user profile on logon, and locking the computer when a screensaver is running. |
| System | A background system process that runs the Windows kernel. |
| svchost.exe with RPCSS | Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Remote Procedure Call (RPC) Service (RPCSS). |
| svchost.exe with Dcom/PnP | Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Distributed Component Object Model (DCOM) and Plug and Play (PnP) services. |

## Service Control (sc)

`sc` can also be used to configure and manage services. Let's experiment with a few commands.

### Query Service

```bash
sc qc wuauserv
```

The `sc qc` command is used to query the service.

### Query Service Over Network

```bash
sc //hostname_or_ip query ServiceName
```

### Start and Stop Services

```bash
sc stop wuauserv
sc start wuauserv
```
