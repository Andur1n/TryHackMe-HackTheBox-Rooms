# TryHackMe - Summit

## 📌 Overview

This project documents a structured malware analysis exercise performed in a sandbox environment. The investigation focused on identifying Indicators of Compromise (IOCs), understanding attacker behaviour, and implementing defensive controls using:

- Endpoint Detection & Response (EDR)
- Firewall rules
- DNS filtering
- Sysmon event analysis
- Sigma rule creation

Across six stages of analysis, multiple malware samples demonstrated techniques including:
- Metasploit-based payload execution
- Command & Control (C2) communication
- Registry-based defense evasion
- Data exfiltration via command-line tools
- Periodic beaconing behaviour

Each stage resulted in the identification of a hidden flag.

## Tools Used

- EDR
- Firewall
- Sigma
- DNS Filtering
- Hash Blocking
- Mitre ATT&CK Mapping

---

## 🧪 Sample 1 – Hash-Based Detection (EDR Blocking)

The first sample (`sample1.exe`) was analysed in a malware sandbox and showed indicators of **Metasploit-related activity** along with suspicious network behaviour.

### 🔍 Indicators of Compromise (IOCs)

To prevent execution, the following file hashes were added to the EDR blocklist:

| Hash Type | Value |
|-----------|------|
| MD5 | `cbda8ae000aa9cbe7c8b982bae006c2a` |
| SHA1 | `83d2791ca93e58688598485aa62597c0ebbf7610` |
| SHA256 | `9c550591a25c6228cb7d74d970d133d75c961ffed2ef7180144859cc09efca8c` |

### 🛡️ Mitigation
- Added file hashes to EDR blocklist
- Detection triggered on single hash match

### 🏁 Result
✔ First flag captured

---

## 🧪 Sample 2 – Network-Based Blocking (Firewall Rule)

The second sample (`sample2.exe`) exhibited **Metasploit-based C2 communication**.

### 🔍 Observed Traffic
- Destination IP: `154.35.10.113`
- Port: `4444`
- Protocol: HTTP-based callback attempt

### 🛡️ Firewall Rule

`Type: Egress
Source IP: Any
Destination IP: 154.35.10.113
Action: Deny`

### 🏁 Result

✔ Second flag captured

## Sample 3 – DNS-Based Command & Control Blocking

The third sample (sample3.exe) demonstrated advanced malicious behaviour including payload retrieval.

### Behaviour

- Trojan activity linked to Metasploit framework
- Download attempt of backdoor.exe
- HTTP communication over port 80

### 🌐 Command & Control Domain
`emudyn.bresonicz.info`

### 🛡️ DNS Rule

`Rule Name: Metasploit Backdoor
Category: Malware
Domain: emudyn.bresonicz.info
Action: Deny`

### 🏁 Result

✔ Third flag captured

## Sample 4 – Registry-Based Defense Evasion

The fourth sample (sample4.exe) focused on system-level manipulation and persistence techniques.

### 🔍 Registry Modifications

`Disable Windows Defender Real-Time Protection
Path: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Defender\Real-Time Protection
Name: DisableRealtimeMonitoring
Value: 1`

`Disable Explorer Balloon Tips
Path: HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced
Name: EnableBalloonTips
Value: 1`

`File Extension Association Check
Path: HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\FileExts\.txt
Name: Progid
Value: txtfile`

### Detection Method
- Sysmon Event Logs → Registry Modifications
- Focused on Defender-related registry tampering
  
### 🛡️ Sigma Rule Classification
- Technique: Defense Evasion
- MITRE ATT&CK: TA0005

### 🏁 Result

✔ Fourth flag captured

## Sample 5 – Beaconing Behaviour Detection (Sigma Rule)

The fifth sample was provided as a log file showing repeating network patterns.

### 🔍 Observed Pattern
- Packet size: 97 bytes
- Interval: 1800 seconds (30 minutes)
- Regular outbound communication
- Likely dynamic C2 backend infrastructure

### 🛡️ Sigma Rule Logic

`Remote IP: Any
Remote Port: Any
Size: 97 bytes
Frequency: 1800 seconds
MITRE ATT&CK: TA0011 (Command & Control)`

### 🏁 Result

✔ Fifth flag captured

##  Sample 6 – Data Exfiltration via Command Execution

The final sample contained a command script used for system reconnaissance and data collection.

## 🔍 Observed Output File
`%temp%\exfiltr8.log`

## 📌 Commands Used by Attacker

Directory enumeration (dir)
Privilege enumeration (net localgroup administrator)
System information (systeminfo, ver)
Network enumeration (ipconfig /all, netstat -ano)
Service enumeration (net start)

## 🛡️ Sigma Rule
`Detection Type: File Creation / Modification
File Path: %temp%
File Name: exfiltr8.log
MITRE ATT&CK: TA0009 (Exfiltration)`

## 🏁 Result

✔ Sixth and final flag captured

### All Flags

- THM{f3cbf08151a11a6a331db9c6cf5f4fe4}
- THM{2ff48a3421a938b388418be273f4806d}
- THM{4eca9e2f61a19ecd5df34c788e7dce16}
- THM{c956f455fc076aea829799c0876ee399}
- THM{46b21c4410e47dc5729ceadef0fc722e}
- THM{c8951b2ad24bbcbac60c16cf2c83d92c}

### Final Verdict

I really enjoyed this room and felt very confident after all I've learned over the last 6 months. It was very cool to see it all come into place and I'm very much looking forward to do more cool things like this.
