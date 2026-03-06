# General

Pentesting methodology 

![0-PT-Process-IG.png](General/0-PT-Process-IG.png)

| **Stage** | **Description** |
| --- | --- |
| `1. Pre-Engagement` | The first step is to create all the necessary documents in the pre-engagement phase, discuss the assessment objectives, and clarify any questions. |
| `2. Information Gathering` | Once the pre-engagement activities are complete, we investigate the company's existing website we have been assigned to assess. We identify the technologies in use and learn how the web application functions. |
| `3. Vulnerability Assessment` | With this information, we can look for known vulnerabilities and investigate questionable features that may allow for unintended actions. |
| `4. Exploitation` | Once we have found potential vulnerabilities, we prepare our exploit code, tools, and environment and test the webserver for these potential vulnerabilities. |
| `5. Post-Exploitation` | Once we have successfully exploited the target, we jump into information gathering and examine the webserver from the inside. If we find sensitive information during this stage, we try to escalate our privileges (depending on the system and configurations). |
| `6. Lateral Movement` | If other servers and hosts in the internal network are in scope, we then try to move through the network and access other hosts and servers using the information we have gathered. |
| `7. Proof-of-Concept` | We create a proof-of-concept that proves that these vulnerabilities exist and potentially even automate the individual steps that trigger these vulnerabilities. |
| `8. Post-Engagement` | Finally, the documentation is completed and presented to our client as a formal report deliverable. Afterward, we may hold a report walkthrough meeting to clarify anything about our testing or results and provide any needed support to personnel tasked with remediating our findings. |

| ☐ Internal Vulnerability Assessment | ☐ External Vulnerability Assessment |
| --- | --- |
| ☐ Internal Penetration Test | ☐ External Penetration Test |
| ☐ Wireless Security Assessment | ☐ Application Security Assessment |
| ☐ Physical Security Assessment | ☐ Social Engineering Assessment |
| ☐ Red Team Assessment | ☐ Web Application Security Assessment |

|  |  |
| --- | --- |
| How many expected live hosts? |  |
| How many IPs/CIDR ranges in scope? |  |
| How many Domains/Subdomains are in scope? |  |
| How many wireless SSIDs in scope? |  |
| How many web/mobile applications? If testing is authenticated, how many roles (standard user, admin, etc.)? |  |
| For a phishing assessment, how many users will be targeted? Will the client provide a list, or we will be required to gather this list via OSINT? |  |
| If the client is requesting a Physical Assessment, how many locations? If multiple sites are in-scope, are they geographically dispersed? |  |
| What is the objective of the Red Team Assessment? Are any activities (such as phishing or physical security attacks) out of scope? |  |
| Is a separate Active Directory Security Assessment desired? |  |
| Will network testing be conducted from an anonymous user on the network or a standard domain user? |  |
| Do we need to bypass Network Access Control (NAC)? |  |

### **Rules of Engagement - Checklist**

| **Checkpoint** | **Contents** |
| --- | --- |
| `☐ Introduction` | Description of this document. |
| `☐ Contractor` | Company name, contractor full name, job title. |
| `☐ Penetration Testers` | Company name, pentesters full name. |
| `☐ Contact Information` | Mailing addresses, e-mail addresses, and phone numbers of all client parties and penetration testers. |
| `☐ Purpose` | Description of the purpose for the conducted penetration test. |
| `☐ Goals` | Description of the goals that should be achieved with the penetration test. |
| `☐ Scope` | All IPs, domain names, URLs, or CIDR ranges. |
| `☐ Lines of Communication` | Online conferences or phone calls or face-to-face meetings, or via e-mail. |
| `☐ Time Estimation` | Start and end dates. |
| `☐ Time of the Day to Test` | Times of the day to test. |
| `☐ Penetration Testing Type` | External/Internal Penetration Test/Vulnerability Assessments/Social Engineering. |
| `☐ Penetration Testing Locations` | Description of how the connection to the client network is established. |
| `☐ Methodologies` | OSSTMM, PTES, OWASP, and others. |
| `☐ Objectives / Flags` | Users, specific files, specific information, and others. |
| `☐ Evidence Handling` | Encryption, secure protocols |
| `☐ System Backups` | Configuration files, databases, and others. |
| `☐ Information Handling` | Strong data encryption |
| `☐ Incident Handling and Reporting` | Cases for contact, pentest interruptions, type of reports |
| `☐ Status Meetings` | Frequency of meetings, dates, times, included parties |
| `☐ Reporting` | Type, target readers, focus |
| `☐ Retesting` | Start and end dates |
| `☐ Disclaimers and Limitation of Liability` | System damage, data loss |
| `☐ Permission to Test` | Signed contract, contractors agreement |

```bash
Projects/
└── Acme Company
    ├── EPT
    │   ├── evidence
    │   │   ├── credentials
    │   │   ├── data
    │   │   └── screenshots
    │   ├── logs
    │   ├── scans
    │   ├── scope
    │   └── tools
    └── IPT
        ├── evidence
        │   ├── credentials
        │   ├── data
        │   └── screenshots
        ├── logs
        ├── scans
        ├── scope
        └── tools
```

Common tools:
`ssh`  - connecting to the services

`netcat`  - connecting to open ports includes banner grabbing

`socat`  - another handy tool for connection and better shells

`tmux`  - multiple terminals

**vim**: [cheatsheet](https://vimsheet.com/)

| **Command** | **Description** |
| --- | --- |
| `x` | Cut character |
| `dw` | Cut word |
| `dd` | Cut full line |
| `yw` | Copy word |
| `yy` | Copy full line |
| `p` | Paste |

| **Command** | **Description** |
| --- | --- |
| `:1` | Go to line number 1. |
| `:w` | Write the file, save |
| `:q` | Quit |
| `:q!` | Quit without saving |
| `:wq` | Write and quit |

### Privilege Escalation Resources

[https://book.hacktricks.wiki/en/index.html](https://book.hacktricks.wiki/en/index.html)

[https://github.com/swisskyrepo/PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)

[https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)

[https://github.com/sleventyeleven/linuxprivchecker](https://github.com/sleventyeleven/linuxprivchecker)

[https://github.com/GhostPack/Seatbelt](https://github.com/GhostPack/Seatbelt)

[https://github.com/411Hall/JAWS](https://github.com/411Hall/JAWS)

[https://github.com/peass-ng/PEASS-ng](https://github.com/peass-ng/PEASS-ng)

[https://gtfobins.org/](https://gtfobins.org/)

[https://lolbas-project.github.io/#](https://lolbas-project.github.io/#)

### Some Ways to Escalate Privileges

1. Check for sudo commands via `sudo -l`
2. Check for `/user/.ssh/id_rsa` — if you have read access, you can get in as root

### File Transfer
`python3 -m http.server 8000`
`scp`

we can use a simple trick to [base64](https://linux.die.net/man/1/base64) encode the file into `base64` format, and then we can paste the `base64` string on the remote server and decode it.
`base64 shell -w 0`

`echo <base64encoded> | base64 -d > shell`