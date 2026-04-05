# 🚨 Indicators of Compromise (IoCs)

## Network Indicators

| Indicator | Type | Description |
|-----------|------|-------------|
| `23.229.162.69` | IP Address | C2 Server — United States |
| `macwinlogistics.in` | Domain | C2 / SMTP Exfiltration Server |
| `bot.whatismyipaddress.com` | Domain | Post-infection IP Lookup |
| `proforma-invoices.com` | Domain | Malware Delivery Domain |

## Host Indicators

| Indicator | Type | Description |
|-----------|------|-------------|
| `tkraw_Protected99.exe` | Filename | HawkEye dropper/stealer binary |
| `BEIJING-5CD1-PC` | Hostname | Compromised victim machine |
| `10.4.10.132` | Internal IP | Victim machine |
| `00:08:02:1c:47:ae` | MAC Address | Victim NIC |
| `roman.mcguire` | Username | Compromised account |

## Malware Properties

| Property | Value |
|----------|-------|
| Family | HawkEye Keylogger |
| Version | Reborn v9 |
| AV Detection | Spyware.HawkEyeKeyLogger |
| Delivery | Phishing email with download link |

## Exfiltration Channel

| Property | Value |
|----------|-------|
| Protocol | SMTP |
| Port | 587 |
| Auth | AUTH LOGIN (Base64) |
| Frequency | Every ~10 minutes |
| SMTP User | roman.mcguire@macwinlogistics.in |
