# Service-Version-Detection-and-OS-Fingerprinting

**Author:** Dashane James  
**Lab Environment:** VMware Workstation | Kali Linux | Metasploitable 2  
**Purpose:** Turn open port data into actionable intelligence: identify exact software versions and map them to real documented vulnerabilities.  
**Status:** 🔵 Completed

---

## 📋 Overview

Knowing that port 21 is open tells you almost nothing on its own. Knowing that port 21 is running vsftpd 2.3.4 tells you there's a documented backdoor with a CVSS score of 9.8. That's the difference version detection makes.

This repository covers two Nmap capabilities that bridge scanning and vulnerability identification: service version detection (`-sV`), which pulls exact software version information from open ports, and OS fingerprinting (`-O`), which attempts to identify the target's operating system from its TCP/IP behavior. It also documents the NVD (National Vulnerability Database) research process used to convert those version strings into real CVEs, which became the basis for all risk rankings in the Week 1 assessment report.

---

## 🧪 Lab Environment

| Component | Details |
|---|---|
| Hypervisor | VMware Workstation (Host-Only Network) |
| Attacker Machine | Kali Linux 2026.1, `192.168.79.129` |
| Target Machine | Metasploitable 2, `192.168.79.130` |
| Network Type | Host-Only (isolated, no internet exposure) |
| Host OS | Windows 11, ASUS Vivobook 14 |

> ⚠️ **Note:** All activity was performed in a controlled, isolated lab environment against a deliberately vulnerable machine. No unauthorized access to live networks was performed.

---

## 🛠️ Tools Used

- **Nmap**: Used for service version detection (`-sV`) and OS fingerprinting (`-O`) against the target
- **NVD (National Vulnerability Database)**: Referenced at nvd.nist.gov to look up CVE details, CVSS scores, and attack vectors for identified software versions

---

## 🔬 Tasks / Assessments Performed

### 1. Run version detection against the top open ports

Version detection works by probing each open port and analyzing the responses (service banners, protocol behavior, and response patterns) to identify what software is actually running and which version. Without it, a port scan only gives you a port number and a generic service name. With `-sV`, you get the specific software version, which is what you actually need to find documented vulnerabilities.

I ran version detection against the five highest-priority ports identified in the previous labs.

```bash
sudo nmap -sV -Pn --disable-arp-ping -n -p 21,22,23,80,3306 192.168.79.130
```

<img width="315" height="176" alt="Screenshot 2026-07-17 233417" src="https://github.com/user-attachments/assets/86953e97-7725-4076-a5b7-963984021e45" />

**Finding:**

The scan returned exact version strings for all five ports:

| Port | Service | Version Identified |
|---|---|---|
| 21/tcp | FTP | vsftpd 2.3.4 |
| 22/tcp | SSH | OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0) |
| 23/tcp | Telnet | Linux telnetd |
| 80/tcp | HTTP | Apache httpd 2.2.8 ((Ubuntu) DAV/2) |
| 3306/tcp | MySQL | MySQL 5.0.51a-3ubuntu5 |

Version detection is the step where reconnaissance becomes intelligence. A port number is a data point. A specific software version is something you can look up, match to a CVE, and evaluate for exploitability.

---

### 2. Run OS fingerprinting and document the result

OS detection works differently from service version detection. Instead of reading banners, Nmap sends a series of specially crafted probes and analyzes how the target's TCP/IP stack responds (things like TTL values, window sizes, and how the target handles unusual packets). It then compares those behaviors against a large database of known OS signatures.

```bash
sudo nmap -O -Pn --disable-arp-ping -n 192.168.79.130
```

<img width="301" height="224" alt="Screenshot 2026-07-18 005604" src="https://github.com/user-attachments/assets/030ccd16-754a-49cd-a945-2edc19c0caad" />

<img width="313" height="229" alt="Screenshot 2026-07-18 005640" src="https://github.com/user-attachments/assets/d85224f5-c097-433b-ad01-f1aa958cacdc" />

**Finding:**

Rather than returning a clean OS match, Nmap reported "No exact OS matches for host" and outputted raw TCP/IP fingerprint data instead. This happens when the target's network stack behavior doesn't closely match any known signature in Nmap's database.

This is actually a useful result to understand. OS fingerprinting isn't always reliable, and when it fails, the raw fingerprint it outputs still contains clues. Cross-referencing it with the service banners from the previous task (which contained strings like "Debian" and "Ubuntu") gave enough context to confirm this was a Linux-based target. In a real assessment, you'd combine all available evidence rather than relying on any single method.

---

### 3. Research CVE details for the highest-risk service version found

With vsftpd 2.3.4 confirmed on port 21, I searched the NVD for that specific version to identify any documented vulnerabilities.

```
Search: nvd.nist.gov → "vsftpd 2.3.4"
```

**Finding:**

The NVD search returned CVE-2011-2523 with a CVSS v3.1 score of **9.8 (Critical)** and a vector of `AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H`.

Breaking down what that vector string means:
- **AV:N**: Network-exploitable. No physical or adjacent network access required.
- **AC:L**: Low attack complexity. No special conditions.
- **PR:N**: No privileges required. Any unauthenticated user can exploit it.
- **UI:N**: No user interaction. The attacker acts alone.
- **C:H / I:H / A:H**: Complete compromise of confidentiality, integrity, and availability.

This CVE describes an intentional backdoor that was planted in the vsftpd 2.3.4 source code. A remote attacker can trigger it to receive a root shell with no credentials. This became the highest-priority finding in the entire Week 1 assessment, and it was later confirmed through live exploitation in the NSE lab.

---

## 📊 Key Findings Summary

| Port/Service | Tool Used | Risk Level | Notes |
|---|---|---|---|
| 21/tcp FTP | Nmap (`-sV`) + NVD | 🔴 Critical | vsftpd 2.3.4, CVE-2011-2523, CVSS 9.8, documented backdoor |
| 23/tcp Telnet | Nmap (`-sV`) | 🔴 Critical | Linux telnetd, transmits all data including credentials in plaintext |
| 80/tcp HTTP | Nmap (`-sV`) | 🟠 High | Apache httpd 2.2.8, end-of-life version with multiple known CVEs |
| 22/tcp SSH | Nmap (`-sV`) | 🟡 Medium | OpenSSH 4.7p1 Debian, outdated; supports deprecated cipher suites |
| 3306/tcp MySQL | Nmap (`-sV`) | 🟠 High | MySQL 5.0.51a, outdated version exposed directly on the network |

**Risk Levels:** 🔴 Critical | 🟠 High | 🟡 Medium | 🟢 Low

---

## 🗺️ MITRE ATT&CK Mapping

| Action Performed | ATT&CK Tactic | Technique ID | Technique Name |
|---|---|---|---|
| Service version detection (`-sV`) | Reconnaissance | T1592 | Gather Victim Host Information |
| OS fingerprinting (`-O`) | Reconnaissance | T1592 | Gather Victim Host Information |
| CVE/CVSS research via NVD | Reconnaissance | T1588.006 | Obtain Capabilities: Vulnerabilities |

---

## 🛡️ Defensive Recommendations

Based on findings, the following remediations would be recommended in a real environment:

1. **vsftpd 2.3.4 backdoor (Critical)**: Uninstall immediately. This version contains a known, publicly documented backdoor and should never run in any environment. Replace with a patched FTP daemon or migrate to SFTP over SSH.
2. **Telnet enabled (Critical)**: Disable Telnet and replace with SSH for all remote administration.
3. **Apache httpd 2.2.8, end of life (High)**: Upgrade to a supported release (2.4.x or later). Suppress version banners in production (ServerTokens Prod). Enforce HTTPS.
4. **MySQL 5.0.51a exposed on the network (High)**: Upgrade to a current supported release. Bind the database to localhost only, a database port should never be accessible directly on a network interface.
5. **OpenSSH 4.7p1, outdated (Medium)**: Upgrade to a current release to eliminate deprecated algorithms and benefit from security patches issued since 2007.

---

## 🔑 Technical Notes

```bash
# Identify software versions on specific open ports
sudo nmap -sV -Pn --disable-arp-ping -n -p 21,22,23,80,3306 192.168.79.130

# Attempt OS fingerprinting
sudo nmap -O -Pn --disable-arp-ping -n 192.168.79.130

# CVE research: search nvd.nist.gov for identified version strings
# Example: search "vsftpd 2.3.4" to find CVE-2011-2523

# Workaround: OS detection may return "No exact OS matches for host"
# instead of a confident guess. When this happens, check the raw
# TCP/IP fingerprint output for platform strings (e.g. x86_64-pc-linux-gnu)
# and cross-reference with service banners (e.g. "Debian", "Ubuntu")
# to confirm the OS type through other available evidence.

# VMware lab note: always add -n to prevent DNS resolution hangs
# on the isolated host-only network.
```

---

## 📌 About This Project

This repository connects service scanning directly to real-world vulnerability research. Version detection turns a port list into a list of specific software versions; NVD research turns those versions into documented CVEs with CVSS scores and attack vector details. That process is what makes reconnaissance actionable.

The vsftpd 2.3.4 backdoor identified here (CVE-2011-2523) was later confirmed through live exploitation in the NSE repository, completing the chain from "port 21 is open" to "root shell confirmed."

**Related repositories (in order):**

- `Nmap-Host-Discovery-and-Lab-Baseline`: Established the initial port list; the five ports analyzed here were identified as open in that baseline
- `Nmap-Scan-Types-SYN-vs-TCP-vs-UDP`: Compared scan types used to build the port list this lab analyzed
- `Wireshark-Capture-and-Analyze-Traffic`: Captured HTTP traffic revealing the Apache version banner and plaintext credentials in the server response
- `TCPDump-CLI-Packet-Capture`: Captured a live Telnet session confirming the plaintext exposure flagged in this lab
- `Nmap-Scripting-Engine`: Used the vsftpd CVE identified here to demonstrate live exploitation, returned uid=0(root)

---

## 👤 Author

**Dashane James**  
Senior Field Service Technician → Cybersecurity Analyst  
📍 Yonkers, NY  
🎓 B.S. Information Technology, SUNY Canton  
🏆 CompTIA Security+  
🔗 [GitHub](https://github.com/Dashanejames1) | [Zero Trust Cyber Security Brand](https://www.instagram.com/zerotrust_cybersecurity)

---

*This repository is part of an active portfolio demonstrating hands-on cybersecurity skills. All lab work performed in isolated environments for educational purposes.*
