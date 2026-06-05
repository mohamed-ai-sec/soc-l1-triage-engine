# SOC L1 Triage Playbooks — Detailed Per-Alert-Type Guide
## TRIAGE MASTER v1.0.0 | Reference File

---

## PLAYBOOK 1 — PHISHING EMAIL ALERT

### What triggers this alert
- Email security gateway (Proofpoint, Mimecast, Microsoft Defender) flags incoming email
- User reports suspicious email
- SIEM detects SPF/DKIM/DMARC failure

### Investigation Checklist
```
[ ] Check SPF result (Pass / Fail / SoftFail)
[ ] Check DKIM signature (Valid / Invalid / Missing)
[ ] Check DMARC policy result
[ ] Analyze sender domain — lookalike? (e.g., micros0ft.com, paypa1.com)
[ ] Check email header — routing hops look normal?
[ ] Inspect links (without clicking) — redirect chain, domain age, hosting country
[ ] Inspect attachment — extension, double extension (.pdf.exe), hash on VT
[ ] Check if other users received same email (campaign)
[ ] Did user click / open / download anything?
[ ] Check proxy/EDR logs for outbound connection post-email
```

### TP Verdict Criteria (any 2+)
- SPF fail + DKIM fail + DMARC reject
- Lookalike domain registered <30 days ago
- Attachment hash detected malicious on VT (2+ engines)
- Link redirects to credential harvesting page
- Multiple users received same email (campaign)
- User clicked AND subsequent suspicious process launched

### FP Verdict Criteria (all must be true)
- Email from known vendor/partner on approved list
- SPF pass or known SPF config issue with legit vendor
- Link is to legitimate business domain
- Attachment is PDF/Word with no macros and clean hash
- No other users flagged same email as suspicious

### MSSP Note
> Always check if the client has a **phishing simulation program** (KnowBe4, Proofpoint Security Awareness). Many "phishing" alerts are internal simulations — but verify with the client before closing.

---

## PLAYBOOK 2 — MALWARE / RANSOMWARE DETECTION

### What triggers this alert
- EDR (CrowdStrike, Defender, SentinelOne) detection
- AV quarantine event
- SIEM hash match against threat intel

### Investigation Checklist
```
[ ] Get the file hash (MD5/SHA256) — submit to VirusTotal
[ ] Check file path — system directory, temp folder, user profile, USB?
[ ] Check parent process — who launched this process?
[ ] Check child processes — what did this process spawn?
[ ] Check command line arguments — encoded? obfuscated?
[ ] Check file creation timestamp — when was it dropped?
[ ] Was the file executed or just detected/quarantined?
[ ] Check for persistence mechanisms (Run keys, scheduled tasks, services)
[ ] Check for network connections made by this process
[ ] For ransomware: check for mass file rename/encryption activity, shadow copy deletion
```

### TP Verdict Criteria
- Hash detected by 3+ AV engines on VT
- Process spawned by unusual parent (e.g., Word → PowerShell → cmd)
- File dropped in %TEMP%, %APPDATA%, or user-writable path
- Encoded command line with -EncodedCommand, IEX, DownloadString
- Network connection to known C2 IP
- Shadow copies deleted (ransomware indicator)
- File execution confirmed (not just detection)

### FP Verdict Criteria
- Hash detected only by 1-2 "noisy" AV engines (heuristic only)
- File is a known legitimate tool flagged generically (PsExec, Nmap, Wireshark)
- AV quarantined BEFORE execution — no lateral spread
- Hash matches known pentest tool during authorized assessment window
- File is in IT software deployment path

### Critical Note — LOLBins
> **Living Off the Land Binaries** (certutil, bitsadmin, mshta, regsvr32, wscript) are legitimate Windows tools frequently abused by malware. Their presence is NOT automatically FP. Check the command line arguments and parent process carefully.

---

## PLAYBOOK 3 — C2 BEACONING / SUSPICIOUS OUTBOUND

### What triggers this alert
- Regular interval outbound connections to same IP/domain
- Proxy alert for connection to newly registered or categorized domain
- Firewall alert for high-frequency external connections
- DNS alert for unusual domain resolution (long/random domain, DGA pattern)

### Investigation Checklist
```
[ ] What is the destination IP/domain? Run on VT, AbuseIPDB, Shodan
[ ] What is the connection interval? (Regular = beaconing; irregular = normal)
[ ] What is the process making the connection?
[ ] What port and protocol? (443 = C2 common; 8080, 4444, 1337 = suspicious)
[ ] What is the data volume? (Small regular = beacon; large = exfil)
[ ] What is the User-Agent string in HTTP(S) traffic?
[ ] When was the domain registered? (<30 days is high risk)
[ ] Is this a known CDN, cloud provider, or SaaS endpoint?
[ ] Is there a business justification for this connection?
[ ] DGA check: does domain look random/high entropy? (e.g., xk3j9plm.ru)
```

### TP Verdict Criteria
- Destination IP in known C2 feed (Abuse.ch, ET Intelligence)
- Connection interval is strikingly regular (±1-2 seconds variance)
- Non-browser process making HTTPS connection (e.g., cmd.exe, powershell.exe)
- Domain registered <7 days ago with no business context
- Low-and-slow data exfiltration pattern (small payloads, regular intervals)
- DGA-pattern domain (high entropy, random-looking)

### FP Verdict Criteria
- Destination is known SaaS (Microsoft O365, Salesforce, Workday, Zoom, etc.)
- Connection from known backup/telemetry agent
- Cloud sync tool (OneDrive, Dropbox) during business hours
- CDN IP (Akamai, Cloudflare, Fastly) — check ASN
- Business app with documented outbound communication pattern

---

## PLAYBOOK 4 — BRUTE FORCE / PASSWORD SPRAY

### What triggers this alert
- SIEM rule: >X failed logins in Y minutes from same source IP
- SIEM rule: Same password tried across multiple accounts (spray)
- Azure AD / Okta alert for suspicious sign-in activity

### Investigation Checklist
```
[ ] Source IP — internal or external? Known or unknown?
[ ] Failure count and time window — how aggressive?
[ ] Did any login SUCCEED after failures? (Critical — lockout bypass)
[ ] Which account(s) targeted? (Admin? Service? Real users?)
[ ] Geolocation of source IP — expected?
[ ] Is this a password spray (1 password → many accounts) or brute force (many passwords → 1 account)?
[ ] Time of day — business hours or after-hours?
[ ] Has this IP appeared before in logs?
[ ] Check if this is from known vulnerability scanner (Nessus, Qualys IP)
[ ] Post-success: any MFA prompts, new device enrollments, suspicious activity?
```

### TP Verdict Criteria
- External source IP with no business justification
- Successful authentication after failure spike (CRITICAL — ESCALATE IMMEDIATELY)
- Password spray pattern across many accounts
- Targeting admin or privileged accounts
- Post-authentication suspicious activity (new inbox rules, OAuth grants)
- Source IP in known bad actor range or proxy/Tor exit node

### FP Verdict Criteria
- Internal IT admin bulk password reset
- Automated service credential misconfiguration (causes repeated failures)
- Vulnerability scanner using credential check module (known IP, change ticket)
- User locked out repeatedly due to old password cached on mobile device
- CI/CD pipeline with service account misconfiguration

---

## PLAYBOOK 5 — LATERAL MOVEMENT

### What triggers this alert
- SMB connections from workstation to workstation (not server-to-workstation)
- Admin share access (C$, ADMIN$, IPC$)
- PsExec, WMI, WinRM, RDP from unexpected source
- Pass-the-Hash / Pass-the-Ticket indicators

### Investigation Checklist
```
[ ] Source of lateral movement — workstation or server?
[ ] Destination — what is the target host's role and criticality?
[ ] Which account is being used? Admin? Service? Standard user?
[ ] Method: SMB, RDP, WMI, PsExec, WinRM?
[ ] Time: during business hours with IT activity, or off-hours?
[ ] Is there a change ticket for admin activity on these hosts?
[ ] Check if source host shows signs of prior compromise (malware, phishing)
[ ] Check for pass-the-hash indicators (NTLM auth with no prior interactive login)
[ ] Check for persistence on destination host post-movement
[ ] Scope: how many hosts involved? (Campaign indicator)
```

### TP Verdict Criteria
- Standard user account performing admin-level remote activity (NO change ticket)
- Workstation-to-workstation admin share access
- PsExec / WMI execution by non-admin account
- Activity occurred AFTER another suspicious event on source host
- Multiple destination hosts (sweeping behavior)
- Off-hours activity with no business justification
- Pass-the-Hash indicators: NTLM auth without interactive logon

### FP Verdict Criteria
- IT admin with documented change request performing maintenance
- Software deployment system (SCCM, Ansible, Puppet) using known service account
- Backup agent performing scheduled backup sweep
- Domain join / provisioning script on known rollout day
- Pentest team activity within authorized window

---

## PLAYBOOK 6 — SUSPICIOUS POWERSHELL / SCRIPT EXECUTION

### What triggers this alert
- EDR detects PowerShell with suspicious flags
- SIEM detects PowerShell with -EncodedCommand, -Exec Bypass
- Script block logging shows obfuscated code

### Investigation Checklist
```
[ ] Full command line — what exactly was executed?
[ ] Is the command Base64 encoded? (Decode it)
[ ] Parent process — what launched PowerShell? (Word, Excel, browser = RED FLAG)
[ ] User running the command — IT admin or standard user?
[ ] Does the script download anything? (IEX + DownloadString = download cradle)
[ ] Does it attempt to disable security tools? (Set-MpPreference -DisableRealtime)
[ ] Does it create persistence? (New-ScheduledTask, Registry modification)
[ ] Is AMSI being bypassed? (Known AMSI bypass strings present)
[ ] Is this a known admin script? Can it be found in IT's script repository?
[ ] Network connection launched by the PowerShell session?
```

### TP Verdict Criteria
- Encoded (-EncodedCommand) + download cradle (IEX/DownloadString)
- Parent process is document viewer or browser (macro/exploit delivery)
- AMSI bypass attempt
- Disabling Windows Defender or other security tools
- Creating persistence (scheduled task, registry Run key)
- Standard user account running privileged operations

### FP Verdict Criteria
- Known IT admin script from documented repository
- Scheduled task with documented purpose and change ticket
- Developer running legitimate script on dev machine (with history)
- RMM tool (ConnectWise, NinjaRMM) executing remote admin commands
- Security team running authorized scripts

---

## PLAYBOOK 7 — DATA EXFILTRATION / DLP TRIGGER

### What triggers this alert
- DLP tool flags large upload to external destination
- SIEM: unusually large outbound traffic
- Proxy alert: upload to non-approved cloud storage

### Investigation Checklist
```
[ ] Destination: known/approved service or unknown external IP?
[ ] Volume: is this volume normal for this user/process?
[ ] Time: business hours or after-hours/weekend?
[ ] User: HR/Finance (sensitive data handlers) or standard user?
[ ] Data type: PII, financial records, IP, credentials?
[ ] Process making the upload: browser, cloud sync, custom tool?
[ ] Was there prior suspicious activity (credential abuse, malware)?
[ ] Check for "staging" activity before exfil (compression, encryption of files)
[ ] Check for recent access to sensitive repositories (DRM, SharePoint, CMDB)
[ ] Is user under HR investigation or recently resigned? (Insider threat vector)
```

### TP Verdict Criteria
- Upload to non-approved external IP or personal cloud storage (Mega, pCloud)
- After-hours activity for user with no travel/remote work justification
- User recently resigned or under HR investigation (insider threat — handle sensitively)
- Large volume + compression + encryption activity = staging for exfiltration
- Prior compromise indicators on same host (malware, credential theft)
- Upload of files matching sensitivity labels (CONFIDENTIAL, PII, RESTRICTED)

### FP Verdict Criteria
- Zoom/Teams video call generating high traffic (check destination domain)
- Approved cloud backup solution (Veeam, Acronis, AWS Backup)
- Developer pushing code to GitHub (approved company repo)
- IT admin uploading diagnostic files to vendor support portal
- Large software patch download via WSUS/SCCM

---

## PLAYBOOK 8 — ANOMALOUS / IMPOSSIBLE TRAVEL LOGIN

### What triggers this alert
- Azure AD / Okta / GSuite flags login from two geographies within short time
- SIEM detects same account logging in from two different countries
- Login from TOR exit node or known VPN

### Investigation Checklist
```
[ ] Two login locations: what are the exact timestamps and geolocations?
[ ] Is travel physically possible in that time window?
[ ] Is user known to use VPN? Which VPN provider?
[ ] Is source IP a known VPN provider IP range? (NordVPN, ExpressVPN, PIA ranges)
[ ] Was MFA completed for both logins?
[ ] New device enrolled during this session?
[ ] Any suspicious post-login activity (email rules, OAuth grants, file access)?
[ ] Was the account recently targeted by phishing or credential stuffing?
[ ] Check for MFA fatigue attack (repeated MFA push notifications)
[ ] Verify directly with user if time permits (don't tip off if insider threat suspected)
```

### TP Verdict Criteria
- Physically impossible travel (e.g., NYC login at 09:00, Moscow login at 09:15)
- Source is known TOR exit node
- No MFA, or MFA was bypassed
- Suspicious post-login activity (inbox rules forwarding email, password reset)
- User confirms they did NOT log in
- Prior credential phishing or credential stuffing activity against this account

### FP Verdict Criteria
- User confirmed they use commercial VPN
- VPN provider IP confirmed (check IP against VPN provider ASN)
- User traveling (HR travel records confirm)
- Shared account used by team in different geolocations (bad practice but common)
- Split-tunnel VPN causing apparent dual location

---

## PLAYBOOK 9 — CLOUD ALERT (AWS / AZURE / GCP)

### What triggers this alert
- CloudTrail / Sentinel / GCP Audit Log flags unusual API call
- S3 bucket public exposure, IAM policy change, security group modification

### Investigation Checklist
```
[ ] Which API call? What resource was affected?
[ ] Which IAM user/role? Is it a human or service role?
[ ] Was MFA used for this API call?
[ ] Source IP: known corporate egress, known developer IP, or unknown?
[ ] Was a security control modified? (SG rule, bucket policy, KMS key)
[ ] Was new IAM user/role/key created?
[ ] Was CloudTrail logging disabled or modified?
[ ] Is there a Terraform/Ansible/IaC change deployment underway?
[ ] Time: CI/CD pipeline window or off-hours?
[ ] Check for privilege escalation in IAM (attaching admin policy to user)
```

### TP Verdict Criteria
- CloudTrail logging disabled or trail deleted (CRITICAL — escalate immediately)
- IAM user created with admin permissions + API key generated in same session
- Security group opened to 0.0.0.0/0 on sensitive ports (22, 3389, 5432)
- S3 bucket ACL changed to public with sensitive data bucket
- API calls from unknown IP with no MFA
- Resources created in unusual region not used by the organization

### FP Verdict Criteria
- Documented IaC deployment (Terraform apply — change ticket)
- CI/CD pipeline using known service role (IAM role, not IAM user)
- Developer with documented admin access in dev/sandbox account
- Security team running authorized audit
- Auto-remediation tool making legitimate configuration corrections

---
*Load this file when investigating a specific alert type. Always cross-reference with the 5-Dimension Context Engine in SKILL.md.*
