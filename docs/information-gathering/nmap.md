---
title: Nmap Scanning
slug: nmap-scanning
excerpt: Network scanning and service enumeration with Nmap
hidden: false
---

# Nmap

Nmap Scripts location: `/usr/share/nmap/scripts/`

Usage: `nmap --script <script name> -p<port> <host>`

## Banner Grabbing

```bash
nmap -sV --script=banner <target>
```

## Basic Scanning

### Most Basic Scan

```bash
nmap 10.129.42.253

Starting Nmap 7.80 ( https://nmap.org ) at 2021-02-25 16:07 EST
Nmap scan report for 10.129.42.253
Host is up (0.11s latency).
Not shown: 995 closed ports
PORT    STATE SERVICE
21/tcp  open  ftp
22/tcp  open  ssh
80/tcp  open  http
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Nmap done: 1 IP address (1 host up) scanned in 2.19 seconds
```

### Detailed Scan with Version Detection

The `-sC` parameter specifies that `Nmap` scripts should be used to try and obtain more detailed information. The `-sV` parameter instructs `Nmap` to perform a version scan.

```bash
nmap -sV -sC -p- 10.129.42.253

Starting Nmap 7.80 ( https://nmap.org ) at 2021-02-25 16:18 EST
Nmap scan report for 10.129.42.253
Host is up (0.11s latency).
Not shown: 65530 closed ports
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed...
```
