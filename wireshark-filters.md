# 🛠 Wireshark Filters & Techniques Used

## Filters Applied During Investigation

```wireshark
# Isolate all traffic from the victim machine
ip.addr == 10.4.10.132

# Identify victim hostname using DHCP hostname field
ip.addr == 10.4.10.132 && dhcp

# Find all HTTP activity (malware download, IP lookup)
http

# Find all SMTP activity (credential exfiltration)
smtp

# Monitor all DNS queries (C2 domain resolution)
dns
```

## Key Menu Paths

| Wireshark Menu | Purpose |
|----------------|---------|
| `Statistics → Capture File Properties` | Total packets, duration, timestamps |
| `Statistics → Conversations → Ethernet` | Layer 2 (MAC-level) activity |
| `Statistics → Conversations → IPv4` | Layer 3 (IP-level) activity |
| `File → Export Objects → HTTP` | Carve files downloaded during capture |
| `Analyze → Follow → TCP Stream` | Read full conversation between two hosts |
| `View → Time Display Format → UTC` | Normalize timestamps to UTC |

## Decoding SMTP Credentials

SMTP AUTH LOGIN sends credentials as Base64 strings.  
Use CyberChef (https://gchq.github.io/CyberChef) with the **"From Base64"** recipe to decode.

Example:
```
Input  : cm9tYW4ubWNndWlyZUBtYWN3aW5sb2dpc3RpY3MuaW4=
Output : roman.mcguire@macwinlogistics.in
```

## Hash Lookup

1. Export the malicious `.exe` via `File → Export Objects → HTTP`
2. Calculate MD5: `md5sum filename.exe` (Linux/Mac) or use HashCalc (Windows)
3. Submit to https://www.virustotal.com for threat intel
