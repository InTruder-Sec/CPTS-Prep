---
title: RPC
deprecated: false
hidden: false
metadata:
  robots: index
---
<br />

The [Remote Procedure Call](https://www.geeksforgeeks.org/remote-procedure-call-rpc-in-operating-system/) (`RPC`) is a concept and, therefore, also a central tool to realize operational and work-sharing structures in networks and client-server architectures. The communication process via RPC includes passing parameters and the return of a function value.

### RPCclient

```
        shellsession
InTruder2sec@htb[/htb]$ rpcclient -U"" 10.129.14.128Enter WORKGROUP\'s password:rpcclient $>
```

The `rpcclient` offers us many different requests with which we can execute specific functions on the SMB server to get information. A complete list of all these functions can be found on the [man page](https://www.samba.org/samba/docs/current/man-html/rpcclient.1.html) of the rpcclient.

| **Query**                 | **Description**                                                    |
| ------------------------- | ------------------------------------------------------------------ |
| `srvinfo`                 | Server information.                                                |
| `enumdomains`             | Enumerate all domains that are deployed in the network.            |
| `querydominfo`            | Provides domain, server, and user information of deployed domains. |
| `netshareenumall`         | Enumerates all available shares.                                   |
| `netsharegetinfo <share>` | Provides information about a specific share.                       |
| `enumdomusers`            | Enumerates all domain users.                                       |
| `queryuser <RID>`         | Provides information about a specific user.                        |

### Brute Forcing User RIDs

```
        shellsession
InTruder2sec@htb[/htb]$ for iin $(seq 500 1100);do rpcclient -N -U"" 10.129.14.128 -c"queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo "";done
```

An alternative to this would be a Python script from [Impacket](https://github.com/SecureAuthCorp/impacket) called [samrdump.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/samrdump.py).

### Impacket - Samrdump.py

```
        shellsession
InTruder2sec@htb[/htb]$ samrdump.py 10.129.14.128
```

The information we have already obtained with `rpcclient` can also be obtained using other tools. For example, the [SMBMap](https://github.com/ShawnDEvans/smbmap) and [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec) tools are also widely used and helpful for the enumeration of SMB services.

### SMBmap

```
        shellsession
InTruder2sec@htb[/htb]$ smbmap -H 10.129.14.128
```

### CrackMapExec

```
        shellsession
InTruder2sec@htb[/htb]$ crackmapexec smb 10.129.14.128 --shares -u'' -p''
```

Another tool worth mentioning is the so-called [enum4linux-ng](https://github.com/cddmp/enum4linux-ng), which is based on an older tool, enum4linux. This tool automates many of the queries, but not all, and can return a large amount of information.

### Enum4Linux-ng - Installation

```
        shellsession
InTruder2sec@htb[/htb]$ git clone https://github.com/cddmp/enum4linux-ng.gitInTruder2sec@htb[/htb]$ cd enum4linux-ngInTruder2sec@htb[/htb]$ pip3 install -r requirements.txt
```

### Enum4Linux-ng - Enumeration

```
        shellsession
InTruder2sec@htb[/htb]$ ./enum4linux-ng.py 10.129.14.128 -A
```