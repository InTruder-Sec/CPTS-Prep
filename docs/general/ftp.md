---
title: FTP Services
slug: ftp
excerpt: File Transfer Protocol enumeration and exploitation techniques
hidden: false
---

# FTP

The `File Transfer Protocol` (`FTP`) is one of the oldest protocols on the Internet. The FTP runs within the application layer of the TCP/IP protocol stack. Thus, it is on the same layer as `HTTP` or `POP`.

## TFTP

`Trivial File Transfer Protocol` (`TFTP`) is simpler than FTP and performs file transfers between client and server processes. However, it `does not` provide user authentication and other valuable features supported by FTP. In addition, while FTP uses TCP, TFTP uses `UDP`, making it an unreliable protocol and causing it to use UDP-assisted application layer recovery.

### TFTP Commands

| **Commands** | **Description** |
| --- | --- |
| `connect` | Sets the remote host, and optionally the port, for file transfers. |
| `get` | Transfers a file or set of files from the remote host to the local host. |
| `put` | Transfers a file or set of files from the local host onto the remote host. |
| `quit` | Exits tftp. |
| `status` | Shows the current status of tftp, including the current transfer mode (ascii or binary), connection status, time-out value, and so on. |
| `verbose` | Turns verbose mode, which displays additional information during file transfer, on or off. |

## vsFTPd Configuration

One of the most used FTP servers on Linux-based distributions is [vsFTPd](https://security.appspot.com/vsftpd.html). The default configuration of vsFTPd can be found in `/etc/vsftpd.conf`.

### Default Configuration

```bash
cat /etc/vsftpd.conf | grep -v "#\|\;"
```

### Configuration Settings

| **Setting** | **Description** |
| --- | --- |
| `listen=NO` | Run from inetd or as a standalone daemon? |
| `listen_ipv6=YES` | Listen on IPv6 ? |
| `anonymous_enable=NO` | Enable Anonymous access? |
| `local_enable=YES` | Allow local users to login? |
| `dirmessage_enable=YES` | Display active directory messages when users go into certain directories? |
| `use_localtime=YES` | Use local time? |
| `xferlog_enable=YES` | Activate logging of uploads/downloads? |
| `connect_from_port_20=YES` | Connect from port 20? |
| `secure_chroot_dir=/var/run/vsftpd/empty` | Name of an empty directory |
| `pam_service_name=vsftpd` | This string is the name of the PAM service vsftpd will use. |
| `rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem` | The last three options specify the location of the RSA certificate to use for SSL encrypted connections. |
| `rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key` |  |
| `ssl_enable=NO` |  |

In addition, there is a file called `/etc/ftpusers` that we also need to pay attention to, as this file is used to deny certain users access to the FTP service.

## Dangerous Settings

There are many different security-related settings we can make on each FTP server. One of these authentication mechanisms is the `anonymous` user. This is often used to allow everyone on the internal network to share files and data without accessing each other's computers.

### Anonymous Login Settings

| **Setting** | **Description** |
| --- | --- |
| `anonymous_enable=YES` | Allowing anonymous login? |
| `anon_upload_enable=YES` | Allowing anonymous to upload files? |
| `anon_mkdir_write_enable=YES` | Allowing anonymous to create new directories? |
| `no_anon_password=YES` | Do not ask anonymous for password? |
| `anon_root=/home/username/ftp` | Directory for anonymous. |
| `write_enable=YES` | Allow the usage of FTP commands: STOR, DELE, RNFR, RNTO, MKD, RMD, APPE, and SITE? |

### Additional Dangerous Settings

| **Setting** | **Description** |
| --- | --- |
| `dirmessage_enable=YES` | Show a message when they first enter a new directory? |
| `chown_uploads=YES` | Change ownership of anonymously uploaded files? |
| `chown_username=username` | User who is given ownership of anonymously uploaded files. |
| `local_enable=YES` | Enable local users to login? |
| `chroot_local_user=YES` | Place local users into their home directory? |
| `chroot_list_enable=YES` | Use a list of local users that will be placed in their home directory? |
| `hide_ids=YES` | All user and group information in directory listings will be displayed as "ftp". |
| `ls_recurse_enable=YES` | Allows the use of recurse listings. |

## FTP Enumeration

### Check vsFTPd Status

```bash
ftp> status

Connected to 10.129.14.136.
No proxy connection.
Connecting using address family: any.
Mode: stream; Type: binary; Form: non-print; Structure: file
Verbose: on; Bell: off; Prompting: on; Globbing: on
Store unique: off; Receive unique: off
Case: off; CR stripping: on
Quote control characters: on
Ntrans: off
Nmap: off
Hash mark printing: off; Use of PORT cmds: on
Tick counter printing: off
```

### Enable Debug Mode

```bash
ftp> debug
Debugging on (debug=1).

ftp> trace
Packet tracing on.

ftp> ls
---> PORT 10,10,14,4,188,195
200 PORT command successful. Consider using PASV.
---> LIST
150 Here comes the directory listing.
```

### Download All Available Files

```bash
wget -m --no-passive ftp://anonymous:anonymous@10.129.14.136
```

### Upload a File

```bash
touch testupload.txt
ftp> put testupload.txt
```

## Nmap FTP Scripts

```bash
find / -type f -name ftp* 2>/dev/null | grep scripts

/usr/share/nmap/scripts/ftp-syst.nse
/usr/share/nmap/scripts/ftp-vsftpd-backdoor.nse
/usr/share/nmap/scripts/ftp-vuln-cve2010-4221.nse
/usr/share/nmap/scripts/ftp-proftpd-backdoor.nse
/usr/share/nmap/scripts/ftp-bounce.nse
/usr/share/nmap/scripts/ftp-libopie.nse
/usr/share/nmap/scripts/ftp-anon.nse
/usr/share/nmap/scripts/ftp-brute.nse
```

## Service Interaction

### Netcat

```bash
nc -nv 10.129.14.136 21
```

### Telnet

```bash
telnet 10.129.14.136 21
```

### OpenSSL (for TLS/SSL)

If the FTP server runs with TLS/SSL encryption, we need a client that can handle TLS/SSL:

```bash
openssl s_client -connect 10.129.14.136:21 -starttls ftp

CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 C = US, ST = California, L = Sacramento, O = Inlanefreight, OU = Dev, CN = master.inlanefreight.htb
```
