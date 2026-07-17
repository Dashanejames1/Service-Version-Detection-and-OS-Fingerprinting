.# Service-Version-Detection-and-OS-Fingerprinting



# 🔍 [Project Title Here]

**Author:** Dashane James  
**Lab Environment:** [e.g. VMware Workstation | Kali Linux | Metasploitable 2]  
**Purpose:** [What is the goal of this lab/project?]  
**Status:** 🟢 Active / 🟡 In Progress / 🔵 Completed

---

## 📋 Overview

[Write 2-3 sentences describing what this project is, what you did, and why. Example: "This repository documents a series of ______ assessments performed in an isolated VMware home lab. The goal was to build hands-on proficiency with ______ aligned with CySA+ exam objectives."] 

This repository identifies and documents all active connections in the lab environment so that we can establish a baseline of the environemnt which we can always use in the future to review or compare any changes. I use several commands in the command line to identify all connections using ping sweep and nmap scan.
---

## 🧪 Lab Environment
| Component | Details |
|---|---|
| Hypervisor | VMware Workstation (Host-Only Network) |
| Attacker Machine | Kali Linux 2026.1 — `192.168.79.129` |
| Target Machine | [Target name] — `192.168.79.130` |
| Network Type | Host-Only (isolated, no internet exposure) |
| Host OS | Windows 11 — ASUS Vivobook 14 |

> ⚠️ **Note:** All activity was performed in a controlled, isolated lab environment against deliberately vulnerable machines. No unauthorized access to live networks was performed.

---

## 🛠️ Tools Used

- **[Tool 1]** — [What it does]
- **[Tool 2]** — [What it does]
- **[Tool 3]** — [What it does]
- **[Tool 4]** — [What it does]

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

###1. [Find all live hosts on the subnet using Ping Sweep]

[Brief description of what you did and why] 
ping sweep to find all live hosts on my subnet to document which hosts are live so we can identify later any unidentifiable live hosts that I must look into.)

# Command used
[sudo nmap -n --disable-arp-ping -sP 192.168.79.0/24]
*-sP is what makes it considered a ping sweep*
image
Output
Command identified 3 live hosts on the subnet: 192.168.79.1 - Gateway/Router for the host-only network(VMwares virtual DHCP server). 192.168.79.130 - The metasploitable 2 target (Target) 192.168.79.129 - Kali (Attacker)

Finding: [What did you discover?] I discovered there are 3 live hosts on this network the Gateway, Metasplotable (target), and Kali VM (ATTACKER). A ping sweep is essentially asking devices on the network "Which devices on this network are alive?" A live host is any active device with an IP address that is powered on, connected and capable of responding to network requests.

2. [Task Name]
[Brief description of what you did and why]

# Command used
[your command here]
Finding: [What did you discover?]

3. [Task Name]
[Brief description of what you did and why]

# Command used
[your command here]
Finding: [What did you discover?]

📊 Key Findings Summary
Port/Service	Tool Used	Risk Level	Notes
[e.g. 21/tcp FTP]	[e.g. Nmap]	🔴 Critical	[Notes]
[e.g. 23/tcp Telnet]	[e.g. Wireshark]	🔴 Critical	[Notes]
[e.g. 80/tcp HTTP]	[e.g. Nikto]	🟠 High	[Notes]
[e.g. 3306/tcp MySQL]	[e.g. OpenVAS]	🟠 High	[Notes]
[e.g. 22/tcp SSH]	[e.g. Nmap]	🟡 Medium	[Notes]
Risk Levels: 🔴 Critical | 🟠 High | 🟡 Medium | 🟢 Low

🗺️ MITRE ATT&CK Mapping
Action Performed	ATT&CK Tactic	Technique ID	Technique Name
[e.g. Port scanning]	[e.g. Reconnaissance]	[e.g. T1595]	[e.g. Active Scanning]
[Action]	[Tactic]	[ID]	[Technique]
[Action]	[Tactic]	[ID]	[Technique]
[Action]	[Tactic]	[ID]	[Technique]
🛡️ Defensive Recommendations
Based on findings, the following remediations would be recommended in a real environment:

[Finding 1] — [Recommendation]
[Finding 2] — [Recommendation]
[Finding 3] — [Recommendation]
[Finding 4] — [Recommendation]
[Finding 5] — [Recommendation]
📚 CySA+ Exam Relevance
This lab directly maps to the following CompTIA CySA+ (CS0-003) exam domains:

Domain	Coverage
Security Operations (33%)	[What this lab covers in this domain]
Vulnerability Management (30%)	[What this lab covers in this domain]
Incident Response (20%)	[What this lab covers in this domain]
Reporting & Communication (17%)	[What this lab covers in this domain]
🔑 Technical Notes
[Any important notes about your lab setup, workarounds, or lessons learned. Example:

"Always add the -n flag to Nmap scans in this VMware environment to prevent DNS resolution hangs."]

-sP and -sT contradict eachother (Can't be used together because -sP means just do a ping/host-discovery sweep, skip ports entirely but -sT tells Nmap to do a full TCP connect port scan. These commands contradict eachother.)

# Any important commands or workarounds
[command here]
📌 About This Project
[1-2 sentences about how this fits into your overall portfolio and career goals.]

Related repositories:

[Repo Name] — [Brief description]
[Repo Name] — [Brief description]
[Repo Name] — Coming soon
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
