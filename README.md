# SOC-Threat-hunting investigation.

---

## 📌 Project Overview

This project documents a full **SOC-investigation** into a credential-theft incident caused by the **HawkEye Keylogger (Reborn v9)** malware. 

Using a captured network traffic file (`.pcap`), I reconstructed the entire attack lifecycle — from initial phishing delivery to data exfiltration — using industry-standard blue team tools. The goal was to identify all **Indicators of Compromise (IoCs)**, trace the attacker's infrastructure, and extract stolen credentials transmitted over the network.

This type of analysis is performed daily by **SOC Analysts, Threat Hunters, and DFIR (Digital Forensics & Incident Response)** professionals in real-world environments.

---

## 🎯 Objectives

- Analyze raw network traffic to identify malicious activity
- Reconstruct the attack chain from infection to data exfiltration
- Extract and document all Indicators of Compromise (IoCs)
- Identify the malware family, version, and C2 infrastructure
- Recover stolen credentials transmitted over the network

---

## 🛠 Tools & Technologies

| Tool | Use Case |
|------|----------|
| **Wireshark** | Deep packet inspection, protocol filtering, stream following |
| **NetworkMiner** | Passive traffic analysis, automatic file & credential extraction |
| **CyberChef** | Decoding Base64-encoded SMTP authentication strings |
| **VirusTotal** | File hash lookup, threat intelligence, AV detection |

---

## 🔬 Technical Skills Demonstrated

- **Protocol Analysis** — TCP, DNS, HTTP, SMTP dissection at packet level
- **Malware Traffic Analysis** — Identifying C2 communication patterns
- **Credential Extraction** — Recovering Base64-encoded credentials from SMTP streams
- **File Carving** — Extracting malicious executables from HTTP streams
- **Threat Intelligence** — Hash-based malware identification via VirusTotal
- **IoC Documentation** — Structured incident reporting of findings

---

## 📂 Project Structure

```
📁 network-forensics-hawkeye/
├── 📄 README.md                    → This document
├── 📄 investigation-report.md      → Full forensic investigation report
├── 📁 iocs/
│   └── iocs.md                     → Indicators of Compromise (IoCs)
├── 📁 analysis/
│   ├── attack-timeline.md          → Reconstructed attack timeline
│   └── protocol-analysis.md        → Protocol-by-protocol breakdown
├── 📁 tools-used/
│   └── wireshark-filters.md        → All filters and techniques used
└── 📁 screenshots/
    └── (analysis screenshots)      → Evidence from the investigation
```

---

## 🧩 Attack Chain Summary

```
[1] DELIVERY
    └─ Victim receives phishing email with fake invoice link

[2] DOWNLOAD
    └─ Victim clicks link → Downloads "tkraw_Protected99.exe"

[3] EXECUTION
    └─ Malware silently installs HawkEye Keylogger v9 on victim machine

[4] RECONNAISSANCE
    └─ Malware contacts bot.whatismyipaddress.com to get victim's public IP

[5] CREDENTIAL HARVESTING
    └─ Keylogger steals stored browser passwords (Chrome, etc.)
    └─ Captures keystrokes and form data

[6] EXFILTRATION
    └─ Every ~10 minutes: stolen data emailed to attacker's SMTP server
    └─ Protocol: SMTP | Port: 587 | Auth: Base64 encoded credentials
    └─ C2 Domain: macwinlogistics.in | C2 IP: 23.229.162.69 (USA)
```

---

## 🚨 Key Findings

### Victim Machine
| Property | Value |
|----------|-------|
| Internal IP | `10.4.10.132` |
| Hostname | `BEIJING-5CD1-PC` |
| MAC Address | `00:08:02:1c:47:ae` |
| Username | `roman.mcguire` |

### Malware
| Property | Value |
|----------|-------|
| Filename | `tkraw_Protected99.exe` |
| Family | HawkEye Keylogger |
| Version | Reborn v9 |
| AV Detection | `Spyware.HawkEyeKeyLogger` (Malwarebytes) |

### Attacker Infrastructure (C2)
| Property | Value |
|----------|-------|
| C2 Domain | `macwinlogistics.in` |
| C2 IP | `23.229.162.69` |
| Location | United States |
| Exfil Method | SMTP (email) |

### Stolen Credentials Recovered
| Platform | Username | Password |
|----------|----------|----------|
| SMTP / Email | `roman.mcguire@macwinlogistics.in` | `P@ssw0rd$` |
| Bank of America | `roman.mcguire` | `P@ssw0rd$` |

---

## 📊 Network Traffic Statistics

| Metric | Value |
|--------|-------|
| Total Packets Captured | 4,003 |
| Capture Start | 2019-04-10 20:37:07 UTC |
| Capture End | 2019-04-10 21:40:48 UTC |
| Total Duration | 1 hour, 3 minutes, 41 seconds |
| Most Active Host (Layer 2) | `00:08:02:1c:47:ae` |
| Most Active Host (Layer 3) | `10.4.10.132` |

---

## 🔑 Key Wireshark Filters Used

```wireshark
# Isolate victim machine traffic
ip.addr == 10.4.10.132

# Identify victim hostname via DHCP
ip.addr == 10.4.10.132 && dhcp

# Find malware download
http

# Find exfiltration traffic
smtp

# DNS C2 resolution
dns
```

---

## 📝 Soc Investigation Summary

The malware was delivered via a spear-phishing email targeting an accountant with a fake invoice. Once executed, the **HawkEye Keylogger Reborn v9** silently harvested saved credentials from the Chrome browser and sent them to the attacker every 10 minutes via SMTP. The stolen data included banking credentials for Bank of America.

The entire attack — from initial compromise to repeated data exfiltration — was completed within a single network session of approximately one hour, demonstrating the speed and stealth of modern keylogger-based credential theft attacks.

---

## 💡 Defensive Recommendations

1. **Email filtering** — Block phishing emails with executable download links
2. **EDR/AV** — Endpoint detection would catch `tkraw_Protected99.exe` on execution
3. **SMTP monitoring** — Alert on outbound SMTP to unusual domains
4. **DNS monitoring** — Block/alert on newly registered domains like `macwinlogistics.in`
5. **Browser hardening** — Disable saved passwords in browsers on corporate machines.

---

## 👤 About Me

** Tripti Pal **  
Aspiring SOC Analyst | Blue Team |CyberSecurity Enthusiast  
Passionate about threat detection, network forensics, and incident response.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat-square&logo=linkedin)](https://linkedin.com/in/yourprofile)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat-square&logo=github)](https://github.com/yourusername)

---

> *"The evidence is always in the packets."*
