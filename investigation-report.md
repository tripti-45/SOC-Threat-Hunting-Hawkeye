# 📋 Forensic Investigation Report

**Case Title:** Credential Theft via Keylogger Malware — Network Traffic Analysis  
**Investigation Type:** Digital Forensics & Incident Response (DFIR)  
**Evidence Type:** Network Packet Capture (PCAP)  
**Analyst:** [Your Name]  
**Date:** April 2025  

---

## 1. Executive Summary

A network traffic capture was analyzed to investigate a suspected credential theft incident. The investigation confirmed that a machine (`BEIJING-5CD1-PC`, IP: `10.4.10.132`) was compromised via a phishing email leading to the download and execution of the **HawkEye Keylogger – Reborn v9** malware.

The malware silently harvested browser-stored credentials and transmitted them to an attacker-controlled SMTP server (`macwinlogistics.in`) every 10 minutes. Stolen credentials included banking login details for Bank of America.

---

## 2. Evidence Overview

| Property | Detail |
|----------|--------|
| Evidence File | `stealer.pcap` |
| Total Packets | 4,003 |
| Capture Window | 2019-04-10 20:37:07 → 21:40:48 UTC |
| Duration | 1 hour, 3 minutes, 41 seconds |
| Primary Analysis Tool | Wireshark |

---

## 3. Phase 1 — Initial Compromise (Phishing & Download)

### 3.1 Phishing Delivery
The accountant received a phishing email with an embedded download link disguised as an invoice. This is a common **Business Email Compromise (BEC)** tactic targeting finance staff.

### 3.2 Malware Download
Filtering HTTP traffic in Wireshark revealed a GET request to a malicious domain. The downloaded executable was:

```
Filename : tkraw_Protected99.exe
Protocol : HTTP
Method   : GET
```

The file was exported from the PCAP via `File → Export Objects → HTTP` and its hash was submitted to VirusTotal, which confirmed:

```
Detection : Spyware.HawkEyeKeyLogger (Malwarebytes)
Family    : HawkEye Keylogger
Version   : Reborn v9
```

---

## 4. Phase 2 — Post-Infection Behavior

### 4.1 External IP Reconnaissance
Immediately after execution, the malware contacted:

```
Domain : bot.whatismyipaddress.com
Purpose: Retrieve the victim's public-facing IP address
```

This is a classic technique used by malware to report the victim's location back to the attacker.

### 4.2 Victim Machine Identification
Using DHCP traffic (`ip.addr == 10.4.10.132 && dhcp` filter), the victim machine details were confirmed:

```
Hostname : BEIJING-5CD1-PC
Username : roman.mcguire
IP       : 10.4.10.132
MAC      : 00:08:02:1c:47:ae
```

---

## 5. Phase 3 — Data Exfiltration via SMTP

### 5.1 C2 Infrastructure
DNS queries after infection revealed the C2 domain:

```
C2 Domain : macwinlogistics.in
C2 IP     : 23.229.162.69
Location  : United States
```

### 5.2 SMTP Credential Extraction
Filtering SMTP traffic and following the TCP stream revealed an `AUTH LOGIN` sequence. The credentials were Base64 encoded:

```
Encoded   : cm9tYW4ubWNndWlyZUBtYWN3aW5sb2dpc3RpY3MuaW4=
Decoded   : roman.mcguire@macwinlogistics.in

Encoded   : UEBzc3cwcmQk
Decoded   : P@ssw0rd$
```

> Decoded using CyberChef → "From Base64" recipe.

### 5.3 Exfiltration Frequency
The malware established an SMTP connection approximately **every 10 minutes**, sending stolen data as email content/attachments.

---

## 6. Phase 4 — Stolen Data Analysis

### 6.1 Email Subject (Malware Metadata)
The SMTP Subject line revealed critical intelligence:

```
Subject: HawkEye Keylogger – Reborn v9 – Passwords Logs – roman.mcguire\BEIJING-5CD1-PC – 173.66.146.112
```

This revealed: malware version, victim username, victim hostname, and attacker's IP.

### 6.2 Recovered Credentials

**Bank of America:**
```
URL      : https://www.bankofamerica.com/
Browser  : Chrome
Username : roman.mcguire
Password : P@ssw0rd$
Field IDs: onlineId1 / passcode1
Captured : 2019-04-10 02:35:17 AM
File     : C:\Users\roman.mcguire\AppData\Local\Google\Chrome\User Data\Default\Login Data
```

---

## 7. Attack Timeline

| Time (UTC) | Event |
|-----------|-------|
| 20:37:07 | Network capture begins |
| ~20:38 | Phishing email link clicked — malware downloaded via HTTP |
| ~20:39 | Malware executes, contacts `bot.whatismyipaddress.com` |
| ~20:40 | DNS query for `macwinlogistics.in` — C2 resolved |
| ~20:41 | First SMTP connection established — credential exfiltration begins |
| Every ~10 min | Repeated SMTP exfiltration cycle |
| 21:40:48 | Capture ends |

---

## 8. Indicators of Compromise (Full List)

See [iocs/iocs.md](../iocs/iocs.md) for the complete IoC table.

---

## 9. Defensive Recommendations

| Priority | Recommendation |
|----------|---------------|
| HIGH | Block outbound SMTP to non-approved mail servers |
| HIGH | Enable email gateway filtering for executable attachments/links |
| HIGH | Deploy EDR to detect keylogger behavior |
| MEDIUM | Implement DNS monitoring / threat intel feed blocking |
| MEDIUM | Disable saved password storage in browsers (GPO) |
| LOW | User security awareness training (phishing simulations) |

---

*Report prepared by [Your Name] | [Date]*
