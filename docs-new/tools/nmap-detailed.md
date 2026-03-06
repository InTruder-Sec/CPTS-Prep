---
title: Nmap - Network Mapper
excerpt: Comprehensive guide to Nmap scanning techniques and capabilities
hidden: false
---

# Nmap

Network Mapper (`Nmap`) is an open-source network analysis and security auditing tool.

## Use Cases

The tool is one of the most used tools by network administrators and IT security specialists. It is used to:

- Audit the security aspects of networks
- Simulate penetration tests
- Check firewall and IDS settings and configurations
- Types of possible connections
- Network mapping
- Response analysis
- Identify open ports
- Vulnerability assessment as well

## Nmap Architecture

Nmap offers many different types of scans that can be used to obtain various results about our targets. Basically, Nmap can be divided into the following scanning techniques:

- Host discovery
- Port scanning
- Service enumeration and detection
- OS detection
- Scriptable interaction with the target service (Nmap Scripting Engine)

**Basic Syntax:**
```bash
nmap <scan types> <options> <target>
```

## Host Discovery

### Scan Network Range

```bash
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5
```

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.0/24` | Target network range. |
| `-sn` | Disables port scanning. |
| `-oA tnet` | Stores the results in all formats starting with the name 'tnet'. |

> **Note:** This scanning method works only if the firewalls of the hosts allow it. Otherwise, we can use other scanning techniques to find out if the hosts are active or not.

### Scanning from a Host List

```bash
sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5
```

| **Scanning Options** | **Description** |
| --- | --- |
| `-sn` | Disables port scanning. |
| `-oA tnet` | Stores the results in all formats starting with the name 'tnet'. |
| `-iL` | Performs defined scans against targets in provided 'hosts.lst' list. |

### Scan Multiple IPs

```bash
sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20 | grep for | cut -d" " -f5
```

Or use IP ranges:

```bash
sudo nmap -sn -oA tnet 10.129.2.18-20 | grep for | cut -d" " -f5
```

### Packet Tracing

```bash
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace
```

## Port Scanning

### TCP Connect Scan

The Nmap [TCP Connect Scan](https://nmap.org/book/scan-methods-connect-scan.html) (`-sT`). The `Connect` scan (also known as a full TCP connect scan) is highly accurate because it completes the three-way TCP handshake, allowing us to determine the exact state of a port (open, closed, or filtered). However, it is not the most stealthy.

### UDP Port Scan

Since `UDP` is a `stateless protocol` and does not require a three-way handshake like TCP, we do not receive any acknowledgment. Consequently, the timeout is much longer, making the whole `UDP scan` (`-sU`) much slower than the `TCP scan` (`-sS`).

```bash
sudo nmap 10.129.2.28 -F -sU

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 16:01 CEST
Nmap scan report for 10.129.2.28
Host is up (0.059s latency).
Not shown: 95 closed ports
PORT     STATE         SERVICE
68/udp   open|filtered dhcpc
137/udp  open          netbios-ns
138/udp  open|filtered netbios-dgm
631/udp  open|filtered ipp
5353/udp open          zeroconf
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

Nmap done: 1 IP address (1 host up) scanned in 98.07 seconds
```

## Saving Results

While we run various scans, we should always save the results. `Nmap` can save the results in 3 different formats:

- Normal output (`-oN`) with the `.nmap` file extension
- Grepable output (`-oG`) with the `.gnmap` file extension
- XML output (`-oX`) with the `.xml` file extension

### Convert XML to HTML

```bash
xsltproc target.xml -o target.html
```

## Nmap Scripting Engine (NSE)

Nmap Scripting Engine (`NSE`) provides us with the possibility to create scripts in Lua for interaction with certain services. There are a total of 14 categories:

| **Category** | **Description** |
| --- | --- |
| `auth` | Determination of authentication credentials. |
| `broadcast` | Scripts for host discovery by broadcasting. |
| `brute` | Executes scripts that try to log in by brute-forcing with credentials. |
| `default` | Default scripts executed by using the `-sC` option. |
| `discovery` | Evaluation of accessible services. |
| `dos` | Check services for denial of service vulnerabilities. |
| `exploit` | Tries to exploit known vulnerabilities for the scanned port. |
| `external` | Scripts that use external services for further processing. |
| `fuzzer` | Identify vulnerabilities through packet fuzzing. |
| `intrusive` | Intrusive scripts that could negatively affect the target system. |
| `malware` | Checks if some malware infects the target system. |
| `safe` | Defensive scripts that do not perform intrusive access. |
| `version` | Extension for service detection. |
| `vuln` | Identification of specific vulnerabilities. |

### NSE Usage

**Default Scripts:**
```bash
sudo nmap <target> -sC
```

**Specific Scripts Category:**
```bash
sudo nmap <target> --script <category>
```

**Defined Scripts:**
```bash
sudo nmap <target> --script <script-name>,<script-name>,...
```

### Aggressive Scan

```bash
sudo nmap <target> -A
```

This scans the target with multiple options: service detection (`-sV`), OS detection (`-O`), traceroute (`--traceroute`), and with the default NSE scripts (`-sC`).

## Performance Optimization

### Timeouts

When Nmap sends a packet, it takes some time (`Round-Trip-Time` - `RTT`) to receive a response from the scanned port. Generally, `Nmap` starts with a high timeout (`--min-RTT-timeout`) of 100ms.

**Default Scan:**
```bash
sudo nmap 10.129.2.0/24 -F
# Nmap done: 256 IP addresses (10 hosts up) scanned in 39.44 seconds
```

**Optimized RTT:**
```bash
sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms
# Nmap done: 256 IP addresses (8 hosts up) scanned in 12.29 seconds
```

### Max Retries

Another way to increase scan speed is by specifying the retry rate of sent packets (`--max-retries`). The default value is `10`, but we can reduce it to `0`. This means if Nmap does not receive a response for a port, it won't send any more packets to that port and will skip it.

### Timing Templates

`Nmap` offers six different timing templates (`-T <0-5>`). These values (`0-5`) determine the aggressiveness of our scans. The default timing template is normal (`-T 3`).

- `T 0` / `T paranoid`
- `T 1` / `T sneaky`
- `T 2` / `T polite`
- `T 3` / `T normal`
- `T 4` / `T aggressive`
- `T 5` / `T insane`

## Firewall/IDS/IPS Evasion

Nmap's TCP ACK scan (`-sA`) method is much harder to filter for firewalls and IDS/IPS systems than regular SYN (`-sS`) or Connect scans (`-sT`) because they only send a TCP packet with only the `ACK` flag.

### Scan by Using Decoys

```bash
sudo nmap 10.129.2.28 -p 80 -sS -Pn -n --disable-arp-ping --packet-trace -D RND:5
```

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.28` | Scans the specified target. |
| `-p 80` | Scans only the specified ports. |
| `-sS` | Performs SYN scan on specified ports. |
| `-Pn` | Disables ICMP Echo requests. |
| `-n` | Disables DNS resolution. |
| `--disable-arp-ping` | Disables ARP ping. |
| `--packet-trace` | Shows all packets sent and received. |
| `-D RND:5` | Generates five random IP addresses that indicates the source IP the connection comes from. |
