# General & RDP

1. **Gettting windows Information**

```powershell
Get-WmiObject -Class win32_OperationSystem | select Version, BuildNumber # gets infromation on OS where win32_OperationSystem is a class instance
```

`Win32_Process` to get a process listing, `Win32_Service` to get a listing of services, and `Win32_Bios` to get [Basic Input/Output System](https://en.wikipedia.org/wiki/BIOS) (`BIOS`) information.

We can use the `ComputerName` parameter to get information about remote computers. `Get-WmiObject` can be used to start and stop services on local and remote computers, and more.

1. **RDP**
    
    Logs/saved profiles Remote Desktop Files (`.rdp`)
    
    **xfreerdp -** Linux tool to use RDP to connect to windows system
    Alternatives: [Remmina](https://remmina.org/) and [rdesktop](http://www.rdesktop.org/),
    
    `xfreerdp /v:<targetIp> /u:htb-student /p:Password`
    

## Windows services & Processes

Windows services are managed via the Service Control Manager (SCM) system, accessible via the `services.msc` MMC add-in

`PS C:\htb> Get-Service | ? {$_.Status -eq "Running"} | select -First 2 |fl` Get Running Services and manage services via Get-Service

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

`Sc` can also be used to configure and manage services. Let's experiment with a few commands.
`sc qc wuauserv` - `sc qc` command is used to query the service.
`sc //hostname or ip of box query ServiceName`  to query over network

We can also use sc to start and stop services.  `sc stop wuauserv`