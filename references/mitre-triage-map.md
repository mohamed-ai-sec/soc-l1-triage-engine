# MITRE ATT&CK → Triage Map
## Common FP Sources Per Technique | TRIAGE MASTER v1.0.0

---

## HOW TO USE THIS FILE
When you identify a MITRE technique match in your alert, look it up here.
The "Common FP Sources" column tells you what LEGITIMATE activity mimics this technique.
The "TP Differentiators" column tells you what separates real attack from false alarm.

---

## INITIAL ACCESS

| Technique | ID | Common FP Sources | TP Differentiators |
|---|---|---|---|
| Phishing | T1566 | Internal awareness campaigns, newsletters, KnowBe4 simulations | SPF/DKIM fail, lookalike domain <30 days, malicious link/attachment confirmed |
| Valid Accounts | T1078 | Admin doing routine work, service account | Login from new geo/device, off-hours, no MFA, post-login anomaly |
| External Remote Services | T1133 | VPN, Citrix, RDP for remote workers | Login from Tor, impossible travel, brute force precedes success |

---

## EXECUTION

| Technique | ID | Common FP Sources | TP Differentiators |
|---|---|---|---|
| PowerShell | T1059.001 | IT admin scripts, RMM tools, SCCM deployment | Encoded commands, download cradle, AMSI bypass, non-admin user |
| Windows Command Shell | T1059.003 | Batch scripts, IT admin work | Launched by Office app, encoded commands, pipe to net/whoami/systeminfo |
| Scripting (VBScript/JavaScript) | T1059.005/007 | Legitimate web apps, WSH scripts | Launched by browser/email client, downloads payload, obfuscated |
| WMI | T1047 | SCCM, monitoring agents, IT admin | Non-standard WMI class queries, lateral movement source, after-hours |
| Scheduled Task | T1053.005 | Legit IT tasks, backup jobs | Newly created, non-standard path, encoded arguments, off-hours creation |

---

## PERSISTENCE

| Technique | ID | Common FP Sources | TP Differentiators |
|---|---|---|---|
| Registry Run Keys | T1547.001 | Software auto-start (Teams, Slack, etc.) | Unusual binary path, obfuscated name, new key not matching installed software |
| Scheduled Tasks | T1053.005 | Windows Update, AV, backup agents | Unknown creator, unusual execution time, non-standard binary |
| New Service Created | T1543.003 | Software installation, IT deployment | Random service name, unusual binary path, created post-compromise indicators |
| Browser Extensions | T1176 | Legit browser extensions | Extension with broad permissions installed silently, unknown publisher |

---

## PRIVILEGE ESCALATION

| Technique | ID | Common FP Sources | TP Differentiators |
|---|---|---|---|
| Token Impersonation | T1134 | IIS/SQL service normal operation | Non-service process impersonating, after suspicious execution chain |
| UAC Bypass | T1548.002 | None — legitimate software uses UAC prompt | Any UAC bypass = high confidence TP |
| Sudo / Admin Access | T1548.003 | IT admin routine operations | Standard user account, no change ticket, new admin group membership |
| Access Token Manipulation | T1134 | None typical | SeImpersonatePrivilege abuse, PrintSpoofer, JuicyPotato patterns |

---

## DEFENSE EVASION

| Technique | ID | Common FP Sources | TP Differentiators |
|---|---|---|---|
| Disable Security Tools | T1562.001 | None legitimate — ALWAYS suspicious | Set-MpPreference -Disable, net stop "security service", tamper protection triggered |
| Obfuscated Files/Information | T1027 | Some legitimate software installers | PowerShell encoded, multilayer obfuscation, XOR/base64 nested |
| Masquerading | T1036 | None legitimate | svchost.exe not in System32, explorer.exe with wrong path, double extension |
| Clear Event Logs | T1070.001 | None legitimate for standard users | wevtutil cl, Clear-EventLog — ESCALATE IMMEDIATELY |
| Indicator Removal | T1070 | None legitimate | File deletion post-execution, log clearing, timestomping |

---

## CREDENTIAL ACCESS

| Technique | ID | Common FP Sources | TP Differentiators |
|---|---|---|---|
| LSASS Memory Dump | T1003.001 | None legitimate — always suspicious | procdump.exe -ma lsass.exe, Task Manager dump of lsass, mimikatz patterns |
| Kerberoasting | T1558.003 | None — always suspicious | RC4 TGS requests for service accounts, SPNs queried in bulk |
| Brute Force | T1110 | IT admin password reset, scanner | External source, after-hours, successful login post-failure |
| Credential Dumping | T1003 | None — always suspicious | SAM dump, NTDS.dit access, lsass access by non-system process |

---

## DISCOVERY

| Technique | ID | Common FP Sources | TP Differentiators |
|---|---|---|---|
| Network Scanning | T1046 | Nessus, Qualys, Nmap in IT ops (known IPs) | Unknown source, after-hours, port sweep pattern, non-scanner host |
| Account Discovery | T1087 | IT admin, helpdesk, HR tools | net user /domain, Get-ADUser run by standard user, bulk enumeration |
| System Information Discovery | T1082 | IT asset management, monitoring agents | whoami /priv, systeminfo run post-exploitation chain, non-admin user |
| File & Directory Discovery | T1083 | Backup agents, search indexers | dir /s /b on sensitive paths, run by non-admin, part of suspicious chain |

---

## LATERAL MOVEMENT

| Technique | ID | Common FP Sources | TP Differentiators |
|---|---|---|---|
| Pass the Hash | T1550.002 | None legitimate | NTLM auth without prior interactive logon, Type 3 logon from workstation |
| Remote Desktop Protocol | T1021.001 | IT admin, remote work | Workstation-to-workstation, non-admin user, off-hours, after compromise |
| SMB / Windows Admin Shares | T1021.002 | IT admin, SCCM, backup agents | Workstation accessing ADMIN$ on another workstation, non-IT user |
| PsExec | T1569.002 | IT admin with change ticket | Standard user, no change ticket, post-malware chain |
| WinRM | T1021.006 | IT admin, automation tools | Standard user, new connection pattern, post-compromise |

---

## COLLECTION

| Technique | ID | Common FP Sources | TP Differentiators |
|---|---|---|---|
| Data Staged | T1074 | Backup staging, software packaging | Compression of sensitive files in temp dir, post-compromise, unusual tool |
| Clipboard Data | T1115 | None commonly flagged | Password manager (if flagged) vs. suspicious process reading clipboard |
| Email Collection | T1114 | PST export by user, IT migration | Unauthorized PST export, bulk mailbox access by non-owner, forwarding rules |

---

## EXFILTRATION

| Technique | ID | Common FP Sources | TP Differentiators |
|---|---|---|---|
| Exfil Over C2 Channel | T1041 | None if C2 confirmed | Encrypted outbound to confirmed C2 = immediate escalation |
| Exfil to Cloud Storage | T1567.002 | OneDrive, Box, Dropbox (approved) | Upload to personal/unapproved storage, unusual volume, sensitive file types |
| Exfil Over Web Service | T1567 | SaaS tools, ticketing systems | Upload to Pastebin, Mega.nz, pCloud, temp file share sites |

---

## COMMAND AND CONTROL

| Technique | ID | Common FP Sources | TP Differentiators |
|---|---|---|---|
| Web Protocols (C2) | T1071.001 | Legitimate HTTPS SaaS traffic | Non-browser User-Agent, regular beacon interval, newly registered domain |
| DNS for C2 | T1071.004 | Normal DNS resolution | High-entropy domain, TXT/NULL record queries, excessive NXDOMAIN |
| Ingress Tool Transfer | T1105 | Software download, patching | certutil -decode, bitsadmin /transfer, wget from non-software domain |
| Remote Access Tools | T1219 | AnyDesk, TeamViewer (approved) | Unauthorized RAT install, unknown tool, off-hours, non-IT user |

---

*Use this map alongside the 5-Dimension Context Engine. A technique match is a hypothesis, not a verdict. Context decides.*
