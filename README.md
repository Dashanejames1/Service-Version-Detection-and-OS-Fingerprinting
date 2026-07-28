..



# 🔍 [# Service-Version-Detection-and-OS-Fingerprinting]

**Author:** Dashane James  
**Lab Environment:** [e.g. VMware Workstation | Kali Linux | Metasploitable 2]  
**Purpose:** [Identify exact software versions on metasploitable and find real CVE's for them.]  .
**Status:** 🟢 Active / 🟡 In Progress / 🔵 Completed

---.
.2.
## 📋 Overview

[Write 2-3 sentences describing what this project is, what you did, and why. Example: "This repository documents a series of ______ assessments performed in an isolated VMware home lab. The goal was to build hands-on proficiency with ______ aligned with CySA+ exam objectives."] 

.
---

## 🧪 Lab Environment
| Component | Details |
|---|---|.2
| Hypervisor | VMware Workstation (Host-Only Network) |
| Attacker Machine | Kali Linux 2026.1 — `192.168.79.129` |
| Target Machine | [Target name] — `192.168.79.130` |
| Network Type | Host-Only (isolated, no internet exposure) |
| Host OS | Windows 11 — ASUS Vivobook 14 |

> ⚠️ **Note:** All activity was performed in a controlled, isolated lab environment against deliberately vulnerable machines. No unauthorized access to live networks was performed.

---

## 🛠️ Tools Used

- **Nmap** — Used for service version detection (`-sV`) and OS fingerprinting (`-O`)
- **NVD (National Vulnerability Database)** — Referenced to research CVE details, CVSS scores, and attack vectors for identified software versions

---

## 📁 Repository Structure

```
[Repo-Name]/
├── README.md
├── [folder-1]/
│   ├── [file1.txt]        # Description of file
│   └── [file2.md]         # Description of file
├── [folder-2]/
│   ├── [file3.md]         # Description of file
│   └── [file4.txt]        # Description of file
└── reports/
    └── [final-report.md]  # Full assessment report
```

---

## 🔬 Tasks / Assessments Performed

###1. [Run version detection on the top open ports to gather more information about the target.]

[I ran Nmap version detection (-sV) against the top open ports I previously identified (21, 22, 23, 80, 3306) to gather more detailed information about the target. Version detection identifies the specific software and version running on each open port, rather than just the generic service name. This is valuable because knowing the exact version allows me to research known, documented vulnerabilities (CVEs) tied to that specific software release.]

# Command used
[sudo nmap -sV -Pn --disable-arp-ping -n -p 21,22,23,80,3306 192.168.79.130]

Output
Command identified the software version of the 5 ports that I specified by port number. FTP port 21 is running software version vsftpd 2.3.4, SSH port 22 is running version OpenSSH 4.7p1 debian 8unbuntu1 (protocol 2.0), Telnet port 23 is running version Linux Telnetd, TCP port 80 is running Apache httpd 2.2.8 ((Ubuntu) Dav/2), and mysql port 3306 is running Mysql 5.0.51a-3ubuntu5.

<img width="315" height="176" alt="Screenshot 2026-07-17 233417" src="https://github.com/user-attachments/assets/86953e97-7725-4076-a5b7-963984021e45" />


Finding: [What did you discover?] I discovered that the -sV command displays the the software version of the service running on the ports that i specified.
version detection bridges from "port is open" to "specific CVE exists".

2. [Run OS detection (-O) and note nmap's guess]
   
I ran Nmap OS detection (-O) against Metasploitable to attempt to identify the target's operating system and kernel. While the previous task identified the specific software running on each service, OS detection provides a different layer of information, the underlying operating system itself, which can reveal OS-level vulnerabilities separate from application-level ones. This information is valuable for identifying OS-specific CVEs, understanding patch levels, and building a more complete picture of the target for further research.


# Command used
[suso nmap -O -Pn --disable-arp-ping -n 192.168.79.130]

Output
After running Nmap OS detection (-O) against Metasploitable. The result was different then expected. Rather than returning a confident OS guess, Nmap reported "No exact OS matches for host" and instead output raw TCP/IP fingerprint data.

<img width="301" height="224" alt="Screenshot 2026-07-18 005604" src="https://github.com/user-attachments/assets/030ccd16-754a-49cd-a945-2edc19c0caad" />

<img width="313" height="229" alt="Screenshot 2026-07-18 005640" src="https://github.com/user-attachments/assets/d85224f5-c097-433b-ad01-f1aa958cacdc" />



Finding: 
-O is a flag used to attempt to identify the operating system of the target host.
I ran Nmap OS detection (-O) against Metasploitable. However, rather than returning a confident OS guess, Nmap reported "No exact OS matches for host" and instead output raw TCP/IP fingerprint data. I found that this happens when the target's network stack behavior doesn't closely match a known signature in Nmap's OS database. This is still a useful result because it shows OS fingerprinting isn't always reliable and that an analyst may need to combine it with other evidence (like port/service data) to determine the target OS with confidence.


### 3. [Research and Document the service version, CVE number, CVSS score, and attack vector for one of the service versions found.]

In this section, I used data I gathered from previous service scans to research real CVE information (CVE number, CVSS score, and attack vectors) on the NVD (National Vulnerability Database) website: nvd.nist.gov for the service version found for port 21 (FTP).

# Command used
[Searched NVD for: vsftpd 2.3.4]

# Output
After searching the NVD database, I found that vsftpd 2.3.4 corresponds to CVE-2011-2523, with a CVSS score of 9.8 (Critical) and an attack vector of `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H`. This indicates the vulnerability is exploitable remotely over the network with low complexity and no privileges required, resulting in a complete compromise of confidentiality, integrity, and availability.

**Finding:** The vsftpd 2.3.4 service running on port 21 (FTP) is affected by CVE-2011-2523, a critical vulnerability (CVSS 9.8) exploitable remotely over the network with low complexity and no privileges required — resulting in complete compromise of confidentiality, integrity, and availability. This CVE corresponds to a known backdoor intentionally planted in this version of vsftpd, historically allowing attackers to gain unauthorized root access to the system.


📊 Key Findings Summary

| Port/Service | Tool Used | Risk Level | Notes |
|---|---|---|---|
| 21/tcp FTP | Nmap (`-sV`) + NVD | 🔴 Critical | vsftpd 2.3.4 — CVE-2011-2523, CVSS 9.8, known backdoor vulnerability |
| 23/tcp Telnet | Nmap (`-sV`) | 🔴 Critical | Linux telnetd — transmits data in plaintext |
| 80/tcp HTTP | Nmap (`-sV`) | 🟠 High | Apache httpd 2.2.8 (Ubuntu) — outdated, multiple known CVEs |
| 22/tcp SSH | Nmap (`-sV`) | 🟡 Medium | OpenSSH 4.7p1 Debian 8ubuntu1 — outdated version |
| 3306/tcp MySQL | Nmap (`-sV`) | 🟠 High | MySQL 5.0.51a-3ubuntu5 — outdated, exposed on network |

**Risk Levels:** 🔴 Critical | 🟠 High | 🟡 Medium | 🟢 Low

🗺️ MITRE ATT&CK Mapping

| Action Performed | ATT&CK Tactic | Technique ID | Technique Name |
|---|---|---|---|
| Service version detection (`-sV`) | Reconnaissance | T1592 | Gather Victim Host Information |
| OS fingerprinting (`-O`) | Reconnaissance | T1592.004 | Gather Victim Host Information: Client Configurations |
| CVE/CVSS research via NVD | Reconnaissance | T1588.006 | Obtain Capabilities: Vulnerabilities |

Defensive Recommendations

1. **vsftpd 2.3.4 backdoor (Critical)** — Upgrade immediately; this version contains a known, publicly documented backdoor and should never be used in any environment.
2. **Telnet in use (Critical)** — Disable Telnet and replace with SSH for all remote administration.
3. **Outdated Apache/PHP versions (High)** — Upgrade to current supported releases to eliminate known CVEs tied to this version.
4. **Outdated MySQL exposed on the network (High)** — Upgrade the database version and restrict access to localhost or an internal-only network segment.
5. **Outdated OpenSSH (Medium)** — Upgrade to a current release to benefit from security patches issued since 2007.


🔑 Technical Notes


"Always add the -n flag to Nmap scans in this VMware environment to prevent DNS resolution hangs."]

-sP and -sT contradict eachother (Can't be used together because -sP means just do a ping/host-discovery sweep, skip ports entirely but -sT tells Nmap to do a full TCP connect port scan. These commands contradict eachother.)

# Any important commands or workarounds

# Identify software versions on specific open ports
sudo nmap -sV -Pn --disable-arp-ping -n -p 21,22,23,80,3306 192.168.79.130

# Attempt OS fingerprinting
sudo nmap -O -Pn --disable-arp-ping -n 192.168.79.130

# Workaround: OS detection may return "No exact OS matches for host" instead of a confident guess. When this happens, check the raw TCP/IP fingerprint output (e.g. platform string like x86_64-pc-linux-gnu) and cross-reference with service banners (e.g. "Linux telnetd", Samba version strings) to confirm the OS through other evidence.



📌 About This Project
[This repository is part of my broader cybersecurity portfolio, connecting service version detection directly to real-world vulnerability research — demonstrating the full process of identifying outdated software and researching its documented risks via the NVD, a process later validated with live exploitation in my Nmap Scripting Engine repository.]

Related repositories:

- `Nmap-Host-Discovery-and-Lab-Baseline` — Established a baseline of the lab network using ping sweeps and host discovery
- `Nmap-Scan-Types-SYN-vs-TCP-vs-UDP` — Compared SYN, TCP connect, and UDP scan types against the target
- `Nmap-Scripting-Engine` — Used the vsftpd CVE identified in this repository to demonstrate live exploitation and confirmed root access
- `Wireshark-Capture-and-Analyze-Traffic` — Captured and analyzed live packet traffic, including plaintext data exposure
- `TCPDump-CLI-Packet-Capture` — Captured traffic from the command line, including a live Telnet session showing plaintext credential exposure

👤 Author
Dashane James
Senior Field Service Technician → Cybersecurity Analyst
📍 Yonkers, NY
🎓 B.S. Information Technology — SUNY Canton
🏆 CompTIA Security+ | CySA+ (In Progress)
🔗 GitHub | Zero Trust Cyber Security Brand

This repository is part of an active portfolio demonstrating hands-on cybersecurity skills. All lab work performed in isolated environments for educational purposes.









**Author:** Dashane James  
**Lab Environment:** VMware Workstation | Kali Linux 2026.1 | Metasploitable 2  
**Purpose:** Hands-on network reconnaissance practice aligned with CompTIA CySA+ (CS0-003) exam objectives  
**Status:** 🟢 Active — updated as lab exercises are completed

---

## 📋 Overview

This repository documents a series of network reconnaissance and port scanning assessments performed in an isolated VMware home lab environment. All scanning activity was conducted against a purposely vulnerable target (Metasploitable 2) on a Host-Only network with no internet exposure.

The goal of this project is to build and demonstrate hands-on proficiency with Nmap — one of the core tools listed in the CompTIA CySA+ exam objectives — while developing real analyst workflows around host discovery, service enumeration, and vulnerability identification.

---

## 🧪 Lab Environment

| Component | Details |
|---|---|
| Hypervisor | VMware Workstation (Host-Only Network) |
| Attacker Machine | Kali Linux 2026.1 — `192.168.79.129` |
| Target Machine | Metasploitable 2 — `192.168.79.130` |
| Network Type | Host-Only (isolated, no internet exposure) |
| Host OS | Windows 11 — ASUS Vivobook 14 |

> ⚠️ **Note:** All scans were performed in a controlled, isolated lab environment against a deliberately vulnerable machine. No unauthorized scanning of any live networks was performed.

---

## 🛠️ Tools Used

- **Nmap 7.98** — Network discovery and port scanning
- **Wireshark** — Packet capture and traffic analysis
- **TCPDump** — CLI-based packet capture
- **Netcat** — Port connectivity verification

---

## 📁 Repository Structure

```
Nmap-Network-Assessment/
├── README.md
├── scans/
│   ├── baseline_fast_scan.txt        # Initial -F fast scan output
│   ├── full_port_scan.txt            # Full -p- all ports scan
│   ├── service_version_scan.txt      # -sV version detection output
│   └── nse_vuln_scan.txt             # NSE script vulnerability results
├── analysis/
│   ├── open_ports_breakdown.md       # Port-by-port analysis and risk notes
│   ├── cve_findings.md               # CVEs identified from version detection
│   └── mitre_attack_mapping.md       # ATT&CK technique mapping
└── reports/
    └── recon_assessment_report.md    # Full analyst-style assessment report
```

---

## 🔬 Assessments Performed

### 1. Host Discovery & Baseline Scan
Performed initial host discovery across the lab subnet and established a baseline of all open ports on the target.

```bash
# Subnet ping sweep
sudo nmap -sT -Pn --disable-arp-ping -n -sP 192.168.79.0/24

# Fast baseline scan — top 100 ports
sudo nmap -sT -Pn --disable-arp-ping -n -F 192.168.79.130 -oN scans/baseline_fast_scan.txt
```

**Key Finding:** 18 open ports identified on initial fast scan including several high-risk services.

---

### 2. Full Port Scan
Comprehensive scan of all 65,535 TCP ports to ensure no services were missed by the fast scan.

```bash
sudo nmap -sT -Pn --disable-arp-ping -n -p- 192.168.79.130 -oN scans/full_port_scan.txt
```

---

### 3. Service Version Detection
Identified exact software versions running on open ports to enable CVE identification.

```bash
sudo nmap -sV -Pn --disable-arp-ping -n -p 21,22,23,80,3306,5432,5900 192.168.79.130 -oN scans/service_version_scan.txt
```

---

### 4. OS Fingerprinting
Attempted OS detection to identify the target operating system.

```bash
sudo nmap -O -Pn --disable-arp-ping -n 192.168.79.130
```

---

### 5. NSE Script Scanning
Used Nmap Scripting Engine to perform targeted vulnerability checks against specific services.

```bash
# Default script scan
sudo nmap -sC -Pn --disable-arp-ping -n -p 21,22,80,445 192.168.79.130

# FTP anonymous login check
sudo nmap --script ftp-anon -Pn --disable-arp-ping -n -p 21 192.168.79.130

# SMB vulnerability check
sudo nmap --script smb-vuln* -Pn --disable-arp-ping -n -p 445 192.168.79.130

# Broad vulnerability scan
sudo nmap --script vuln -Pn --disable-arp-ping -n -p 21,22,80 192.168.79.130 -oN scans/nse_vuln_scan.txt
```

---

## 📊 Key Findings Summary

| Port | Service | Version | Risk Level | Notes |
|---|---|---|---|---|
| 21/tcp | FTP | vsftpd 2.3.4 | 🔴 Critical | Known backdoor vulnerability (CVE-2011-2523) |
| 22/tcp | SSH | OpenSSH 4.7p1 | 🟡 Medium | Outdated version |
| 23/tcp | Telnet | Linux telnetd | 🔴 Critical | Plaintext credential transmission |
| 80/tcp | HTTP | Apache 2.2.8 | 🔴 Critical | Outdated, multiple known CVEs |
| 139/tcp | NetBIOS | Samba smbd | 🟠 High | SMB exposure risk |
| 445/tcp | SMB | Samba smbd 3.X | 🟠 High | Legacy SMB vulnerabilities |
| 3306/tcp | MySQL | MySQL 5.0.51a | 🔴 Critical | Database exposed on network |
| 5432/tcp | PostgreSQL | PostgreSQL 8.3 | 🟠 High | Database exposed on network |
| 5900/tcp | VNC | Protocol 3.3 | 🔴 Critical | Remote desktop with weak auth |

---

## 🗺️ MITRE ATT&CK Mapping

| Action Performed | ATT&CK Tactic | Technique ID | Technique Name |
|---|---|---|---|
| Nmap host discovery | Reconnaissance | T1595 | Active Scanning |
| Port scanning | Reconnaissance | T1595.001 | Scanning IP Blocks |
| Service version detection | Reconnaissance | T1592 | Gather Victim Host Info |
| NSE script scanning | Reconnaissance | T1595.002 | Vulnerability Scanning |

---

## 🛡️ Defensive Recommendations

Based on findings, the following remediations would be recommended in a real environment:

1. **Disable Telnet (port 23)** — Replace with SSH immediately. Telnet transmits credentials in plaintext.
2. **Patch or replace vsftpd** — CVE-2011-2523 is a critical backdoor. Upgrade or use SFTP.
3. **Restrict database ports** — MySQL (3306) and PostgreSQL (5432) should never be exposed directly to a network. Bind to localhost only.
4. **Disable VNC or enforce strong authentication** — VNC Protocol 3.3 has no encryption. Use a VPN or SSH tunnel.
5. **Update Apache** — Version 2.2.8 is end-of-life with numerous critical CVEs. Upgrade to current release.
6. **Restrict SMB access** — Apply host-based firewall rules to limit SMB (445) to authorized hosts only.

---

## 📚 CySA+ Exam Relevance

This lab directly maps to the following CompTIA CySA+ (CS0-003) exam domains:

| Domain | Coverage |
|---|---|
| Security Operations (33%) | Network scanning, host discovery, traffic analysis |
| Vulnerability Management (30%) | CVE identification, CVSS scoring, risk prioritization |
| Incident Response (20%) | Understanding attack surface for IR planning |
| Reporting & Communication (17%) | Documented findings and remediation recommendations |

---

## 🔑 Technical Notes

> **VMware Lab Note:** Nmap scans in this VMware Host-Only environment hang indefinitely when DNS resolution is enabled. The `-n` flag (disable DNS resolution) is required for all scans in this lab setup. This is a known VMware Host-Only network behavior and does not affect real-world scan performance.

**Working scan command for this lab:**
```bash
sudo nmap -sT -Pn --disable-arp-ping -n -F [target IP]
```

---

## 📌 About This Project

This lab is part of an ongoing cybersecurity portfolio being built during active CySA+ exam preparation. Additional repositories covering Wireshark analysis, Splunk SIEM detection, OpenVAS vulnerability scanning, and incident response simulations will be added as they are completed.

**Related repositories:**
- `CySA-Home-Lab` — Full 30-day lab plan with all tools and exercises
- `Splunk-SIEM-Lab` — Coming soon
- `OpenVAS-Vulnerability-Assessment` — Coming soon

---

## 👤 Author

**Dashane James**  
Senior Field Service Technician → Cybersecurity Analyst  
📍 Yonkers, NY  
🎓 B.S. Information Technology — SUNY Canton  
🏆 CompTIA Security+ | CySA+ (In Progress)  
🔗 [GitHub](https://github.com/Dashanejames1) | [Zero Trust Cyber Security Brand](https://www.instagram.com/zerotrust_cybersecurity)

---

*This repository is par
