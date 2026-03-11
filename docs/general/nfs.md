---
title: NFS
deprecated: false
hidden: false
metadata:
  robots: index
---
`Network File System` (`NFS`) is a network file system developed by Sun Microsystems and has the same purpose as SMB. Its purpose is to access file systems over a network as if they were local. However, it uses an entirely different protocol.

| **Version** | **Features**                                                                                                                                                                                                                                                         |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `NFSv2`     | It is older but is supported by many systems and was initially operated entirely over UDP.                                                                                                                                                                           |
| `NFSv3`     | It has more features, including variable file size and better error reporting, but is not fully compatible with NFSv2 clients.                                                                                                                                       |
| `NFSv4`     | It includes Kerberos, works through firewalls and on the Internet, no longer requires portmappers, supports ACLs, applies state-based operations, and provides performance improvements and high security. It is also the first version to have a stateful protocol. |

A significant advantage of NFSv4 over its predecessors is that only one UDP or TCP port `2049` is used to run the service, which simplifies the use of the protocol across firewalls.

NFS is based on the [Open Network Computing Remote Procedure Call](https://en.wikipedia.org/wiki/Sun_RPC) (`ONC-RPC`/`SUN-RPC`) protocol exposed on `TCP` and `UDP` ports `111`, which uses [External Data Representation](https://en.wikipedia.org/wiki/External_Data_Representation) (`XDR`) for the system-independent exchange of data. The NFS protocol has `no` mechanism for `authentication` or `authorization`. Instead, authentication is completely shifted to the RPC protocol's options. The authorization is derived from the available file system information.

# Default Configuration

NFS is not difficult to configure because there are not as many options as FTP or SMB have. The `/etc/exports` file contains a table of physical filesystems on an NFS server accessible by the clients. The [NFS Exports Table](http://manpages.ubuntu.com/manpages/trusty/man5/exports.5.html) shows which options it accepts and thus indicates which options are available to us.

### Exports File

```
        shellsession
InTruder2sec@htb[/htb]$ cat /etc/exports# /etc/exports: the access control listfor filesystems which may be exported#               to NFS clients.  See exports(5).## Examplefor NFSv2 and NFSv3:# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)## Examplefor NFSv4:# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
```

| **Option**         | **Description**                                                                                                                             |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `rw`               | Read and write permissions.                                                                                                                 |
| `ro`               | Read only permissions.                                                                                                                      |
| `sync`             | Synchronous data transfer. (A bit slower)                                                                                                   |
| `async`            | Asynchronous data transfer. (A bit faster)                                                                                                  |
| `secure`           | Ports above 1024 will not be used.                                                                                                          |
| `insecure`         | Ports above 1024 will be used.                                                                                                              |
| `no_subtree_check` | This option disables the checking of subdirectory trees.                                                                                    |
| `root_squash`      | Assigns all permissions to files of root UID/GID 0 to the UID/GID of anonymous, which prevents `root` from accessing files on an NFS mount. |

### ExportFS

```
        shellsession
root@nfs:~# echo'/mnt/nfs  10.129.14.0/24(sync,no_subtree_check)' >> /etc/exportsroot@nfs:~# systemctl restart nfs-kernel-serverroot@nfs:~# exportfs/mnt/nfs        10.129.14.0/24
```

We have shared the folder `/mnt/nfs` to the subnet `10.129.14.0/24` with the setting shown above. This means that all hosts on the network will be able to mount this NFS share and inspect the contents of this folder.

***

# Dangerous Settings

However, even with NFS, some settings can be dangerous for the company and its infrastructure. Here are some of them listed:

| **Option**       | **Description**                                                                                                      |
| ---------------- | -------------------------------------------------------------------------------------------------------------------- |
| `rw`             | Read and write permissions.                                                                                          |
| `insecure`       | Ports above 1024 will be used.                                                                                       |
| `nohide`         | If another file system was mounted below an exported directory, this directory is exported by its own exports entry. |
| `no_root_squash` | All files created by root are kept with the UID/GID 0.                                                               |

<br />

Show Available NFS Shares
`shellsession
InTruder2sec@htb[/htb]$ showmount -e 10.129.14.128`

Export list for 10.129.14.128:
/mnt/nfs 10.129.14.0/24

Mounting NFS Share
`shellsession
InTruder2sec@htb[/htb]$ mkdir target-NFS
InTruder2sec@htb[/htb]$ sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock
InTruder2sec@htb[/htb]$ cd target-NFS
InTruder2sec@htb[/htb]$ tree .`

<br />

List Contents with Usernames & Group Names
shellsession
InTruder2sec@htb[/htb]$ ls -l mnt/nfs/

```
total 16
-rw-r--r-- 1 cry0l1t3 cry0l1t3 1872 Sep 25 00:55 cry0l1t3.priv
-rw-r--r-- 1 cry0l1t3 cry0l1t3  348 Sep 25 00:55 cry0l1t3.pub
-rw-r--r-- 1 root     root     1872 Sep 19 17:27 id_rsa
-rw-r--r-- 1 root     root      348 Sep 19 17:28 id_rsa.pub
-rw-r--r-- 1 root     root        0 Sep 19 17:22 nfs.share
```

List Contents with UIDs & GUIDs

```
InTruder2sec@htb[/htb]$ ls -n mnt/nfs/

total 16
-rw-r--r-- 1 1000 1000 1872 Sep 25 00:55 cry0l1t3.priv
-rw-r--r-- 1 1000 1000  348 Sep 25 00:55 cry0l1t3.pub
-rw-r--r-- 1    0 1000 1221 Sep 19 18:21 backup.sh
-rw-r--r-- 1    0    0 1872 Sep 19 17:27 id_rsa
-rw-r--r-- 1    0    0  348 Sep 19 17:28 id_rsa.pub
-rw-r--r-- 1    0    0    0 Sep 19 17:22 nfs.share


```

It is important to note that if the root_squash option is set, we cannot edit the backup.sh file even as root.

We can also use NFS for further escalation. For example, if we have access to the system via SSH and want to read files from another folder that a specific user can read, we would need to upload a shell to the NFS share that has the SUID of that user and then run the shell via the SSH user.

Unmounting

```
shellsession
InTruder2sec@htb[/htb]$ cd ..
InTruder2sec@htb[/htb]$ sudo umount ./target-NFS
```
