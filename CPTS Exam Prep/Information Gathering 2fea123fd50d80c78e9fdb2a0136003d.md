# Information Gathering

- Open-Source Intelligence
- Infrastructure Enumeration
- Service Enumeration
- Host Enumeration

**SMB**

SMB allows users and administrators to share folders and make them accessible remotely by other users. Often these shares have files in them that contain sensitive information such as passwords. A tool that can enumerate and interact with SMB shares is [smbclient](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html). The `-L` flag specifies that we want to retrieve a list of available shares on the remote host, while `-N` suppresses the password prompt.

Service Scanning

```bash
InTruder2sec@htb[/htb]$ smbclient -N -L \\\\10.129.42.253

	Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	users           Disk
	IPC$            IPC       IPC Service (gs-svcscan server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available
```

### **SNMP**

SNMP Community strings provide information and statistics about a router or device, helping us gain access to it. The manufacturer default community strings of `public` and `private` are often unchanged. In SNMP versions 1 and 2c, access is controlled using a plaintext community string, and if we know the name, we can gain access to it. Encryption and authentication were only added in SNMP version 3. Much information can be gained from SNMP.

```powershell
InTruder2sec@htb[/htb]$ snmpwalk -v 2c -c public 10.129.42.253 1.3.6.1.2.1.1.5.0

iso.3.6.1.2.1.1.5.0 = STRING: "gs-svcscan"
```

Service Scanning

```bash
InTruder2sec@htb[/htb]$ snmpwalk -v 2c -c private  10.129.42.253

Timeout: No Response from 10.129.42.253

```

A tool such as [onesixtyone](https://github.com/trailofbits/onesixtyone) can be used to brute force the community string names using a dictionary file of common community strings such as the `dict.txt` file included in the GitHub repo for the tool.

Service Scanning

```bash
InTruder2sec@htb[/htb]$ onesixtyone -c dict.txt 10.129.42.254

Scanning 1 hosts, 51 communities
10.129.42.254 [public] Linux gs-svcscan 5.4.0-66-generic#74-Ubuntu SMP Wed Jan 27 22:54:38 UTC 2021 x86_64
```

[Nmap](Information%20Gathering/Nmap%202fea123fd50d80448fa9e8ad794c0257.md)

[Footprinting](Information%20Gathering/Footprinting%20317a123fd50d8030bd7fecfef01ad752.md)