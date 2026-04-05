# SOC-Threat-Hunting-Hawkeye
SOC threat hunting investigation analyzing suspicious system activity using Windows Event Logs with MITRE ATT&amp;CK mapping.

## **📌 Project Overview**
This project documents a full network forensic investigation into a credential-theft incident caused by the HawkEye Keylogger (Reborn v9) malware.
Using a captured network traffic file (.pcap), I reconstructed the entire attack lifecycle — from initial phishing delivery to data exfiltration — using industry-standard blue team tools. The goal was to identify all Indicators of Compromise (IoCs), trace the attacker's infrastructure, and extract stolen credentials transmitted over the network.
This type of analysis is performed daily by SOC Analysts, Threat Hunters, and DFIR (Digital Forensics & Incident Response) professionals in real-world environments.

## **🎯 Objectives**
- Analyze raw network traffic to identify malicious activity
- Reconstruct the attack chain from infection to data exfiltration
- Extract and document all Indicators of Compromise (IoCs)
- Identify the malware family, version, and C2 infrastructure
- Recover stolen credentials transmitted over the network
