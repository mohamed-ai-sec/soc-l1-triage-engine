---
name: soc-l1-triage-engine
version: 1.0.0
author: Mohamed Benbouazza
tags:
  - soc
  - triage
  - l1-analyst
  - detection-engineering
  - mssp
  - threat-intelligence
  - mitre-attack
  - incident-response
description: >
  Elite SOC L1 True Positive / False Positive Verdict Engine — TRIAGE MASTER v1.0.0.
  ALWAYS trigger when an analyst pastes any alert, SIEM event, EDR detection, firewall block, phishing report, suspicious process, or raw log and asks if it is real or noise.
  Covers: phishing, malware, ransomware, C2 beaconing, brute force, credential abuse, lateral movement, exfiltration, privilege escalation, suspicious PowerShell, anomalous login, DLP, and cloud alerts.
  Uses a 5-Dimension Context Engine, per-alert-type playbooks, MITRE ATT&CK mapping, and a confidence-scored verdict framework — built for MSSP SLA compliance and client trust.
trigger_phrases:
  - "is this TP or FP?"
  - "should I escalate?"
  - "help me triage"
  - "is this malicious?"
  - "close or escalate?"
  - "analyze this event"
  - "is this suspicious?"
  - "triage this alert"
  - "is this a false positive?"
  - "is this a true positive?"
output_format: structured-verdict-report
---

# TRIAGE MASTER — SOC L1 True Positive / False Positive Verdict Engine
## v1.0.0 | Built for MSSP-Grade Accuracy | SOC Best Practices Methodology

---

## MISSION

Every alert you receive is either a real threat (True Positive) or noise (False Positive). Your job as L1 is to reach the right verdict — confidently, quickly, and with documented evidence. A missed TP costs your client everything. An FP storm costs them trust. This skill gives you a structured framework aligned with senior analyst methodology.

**Never guess. Never assume. Evidence decides.**

---

## PHASE 0 — ALERT INTAKE

When an analyst shares an alert, immediately extract these fields. If any are missing, ask:

```
ALERT INTAKE CHECKLIST
──────────────────────
[ ] Alert Name / Rule Name
[ ] Alert Severity (Critical / High / Medium / Low)
[ ] Timestamp (and timezone)
[ ] Source IP / Hostname
[ ] Destination IP / Domain / URL
[ ] User Account involved
[ ] Process / File / Hash (if any)
[ ] Raw log snippet (if available)
[ ] SIEM / Tool generating the alert (Splunk, QRadar, Sentinel, CrowdStrike, etc.)
[ ] Client environment context (if MSSP)
```

> **PRO TIP (Senior Analyst):** The alert name is a hypothesis, NOT a verdict. The raw log is the truth. Always look at the raw log first.

---

## PHASE 1 — THE 5-DIMENSION CONTEXT ENGINE

Before making any verdict, you MUST evaluate these 5 dimensions. Each dimension either increases or decreases confidence in TP/FP.

### DIMENSION 1: USER CONTEXT
Ask / verify:
- Who is this user? (role, department, seniority)
- Is this behavior **expected for their role**? (admin running PowerShell = normal; finance user running PowerShell = suspicious)
- Has this user triggered this alert **before**? (historical pattern)
- Is the user currently on vacation, off-hours, or in an unusual location?
- Is the account a **service account, system account, or shared account**? (often FP sources)

**TP Signal:** Account is a standard user, first time this behavior, off-hours, anomalous location
**FP Signal:** Known admin/service account, scheduled task account, repeated benign history

---

### DIMENSION 2: ASSET CRITICALITY
Ask / verify:
- What is this asset? (workstation, server, DC, POS terminal, OT device, jump box)
- What data does it hold or have access to? (PII, financial, IP, AD)
- Is this asset on a **sensitivity tier** (Crown Jewel, Tier-1, standard)?
- Is this a **developer/test machine** (higher FP likelihood) or production?

**TP Signal:** High-value asset (DC, finance server, backup server), production environment
**FP Signal:** Test/dev machine, known noisy endpoint, vulnerability scanner host

---

### DIMENSION 3: BEHAVIORAL BASELINE
Ask / verify:
- Is this activity **normal for this asset or user** historically? (UEBA baseline)
- What happened **30 minutes BEFORE** the alert? (pre-attack context)
- What happened **30 minutes AFTER** the alert? (post-action indicators)
- Is there a **business justification**? (scheduled job, change ticket, pentesting window)

**TP Signal:** No prior history of this behavior, no change ticket, unusual timing
**FP Signal:** Recurs daily at same time (scheduled task), known pentest window, documented maintenance

---

### DIMENSION 4: IOC ENRICHMENT
Check every indicator against:
- **VirusTotal** — hash, IP, domain reputation
- **AbuseIPDB** — IP abuse history
- **AlienVault OTX / MISP** — threat intel feeds
- **Shodan / Censys** — what is the destination actually running?
- **MXToolbox** — email header/SPF/DKIM validation
- **Internal blocklists / allowlists** — is this already known?

**TP Signal:** IOC matches known malicious intel, high detection ratio on VT, Tor/VPN/bulletproof hosting
**FP Signal:** Whitelisted internal tool, known CDN (Akamai, Cloudflare, AWS), legitimate SaaS

---

### DIMENSION 5: ATTACK CHAIN CORRELATION
Ask / verify:
- Does this alert **fit into a kill chain**? (Recon → Delivery → Exploitation → Installation → C2 → Actions)
- Are there **correlated alerts** for the same host/user in the last 24-48 hours?
- Does this match a **MITRE ATT&CK technique** that maps to a known campaign?
- Is there **lateral movement** involvement (multiple hosts, internal scanning)?

**TP Signal:** Multiple correlated alerts, fits MITRE technique, campaign pattern visible
**FP Signal:** Isolated single alert, no correlated events, no subsequent suspicious activity

---

## PHASE 2 — ALERT TYPE PLAYBOOKS

Select the playbook matching the alert type. Each playbook gives the **key questions** and **TP vs FP signatures**.

> For detailed playbooks see: `references/playbooks.md`

### Quick Reference — Alert Type Decision Matrix

| Alert Type | Top TP Indicators | Top FP Indicators |
|---|---|---|
| **Phishing Email** | SPF/DKIM fail, lookalike domain, malicious attachment/link, urgency language | Internal newsletter, pentest phish, known mail vendor, mailing list |
| **Malware / Ransomware** | Known hash on VT, suspicious parent process, encoded commands, file encryption activity | AV scan quarantine file, legitimate tool (PsExec, Mimikatz in pentest), false detection on packer |
| **C2 Beaconing** | Regular interval outbound, beacon to known C2 IP/domain, non-browser User-Agent, encrypted traffic to new domain | Cloud backup tool (Dropbox, OneDrive), telemetry agent (Splunk UF, antivirus), CDN |
| **Brute Force / Password Spray** | High failure count + success from same IP, external IP, off-hours, geolocation anomaly | Internal IT admin reset activity, automated service, vulnerability scanner, login testing |
| **Credential Abuse / Impossible Travel** | Login from 2 countries within minutes, unfamiliar geolocation, no MFA, new device | VPN endpoint change, travel confirmed, legitimate geo-split for remote worker |
| **Lateral Movement** | SMB/RDP from workstation to workstation, admin share access, PsExec/WMI use by standard user | IT admin maintenance, domain join script, backup agent, software deployment tool |
| **Suspicious PowerShell** | Encoded base64, -Exec Bypass, download cradle (IEX/DownloadString), AMSI bypass | Known admin script, scheduled task (documented), developer with legitimate need |
| **Data Exfiltration** | Large upload to unknown external IP, cloud storage not approved, after-hours, DLP trigger | Zoom/Teams video call, approved backup, authorized cloud sync, developer pushing to GitHub |
| **Privilege Escalation** | Token impersonation, UAC bypass, known exploit process chain, new admin account created | IT admin performing legitimate elevation, documented change request, AD provisioning |
| **Anomalous Login** | New country, new device, after-hours, no MFA, service account login to interactive shell | VPN provider IP pool change, employee travel, IT admin using service account (bad practice but common) |

---

## PHASE 3 — THE VERDICT DECISION TREE

Work through this tree in order. Stop at the first DEFINITIVE answer. If still ambiguous, reach a CONFIDENCE SCORE.

```
START
│
├─► Is this IOC (IP/hash/domain) confirmed malicious by 2+ trusted TI sources?
│   YES → STRONG TP → Proceed to Phase 4 (Escalation)
│   NO  → Continue ↓
│
├─► Is there a documented business justification or change ticket for this activity?
│   YES → LIKELY FP → Verify and document, proceed to Phase 5 (FP Closure)
│   NO  → Continue ↓
│
├─► Is the user/asset behavior consistent with historical baseline?
│   YES (consistent) → LEAN FP → Check other dimensions
│   NO (anomalous)   → LEAN TP → Check other dimensions
│
├─► Are there 2+ correlated alerts for the same host/user in the last 24h?
│   YES → ESCALATE AS TP (campaign pattern)
│   NO  → Continue ↓
│
├─► Does the activity match a MITRE ATT&CK technique with a plausible attack narrative?
│   YES + high-value asset → TP — Escalate
│   YES + low-value asset  → Medium confidence — gather more evidence
│   NO                     → FP likelihood increases
│
└─► CONFIDENCE SCORE VERDICT (see below)
```

### Confidence Score Model

After evaluating all 5 dimensions, score each:

| Dimension | TP Evidence (+1) | Neutral (0) | FP Evidence (-1) |
|---|---|---|---|
| User Context | Anomalous for role | Normal user, no history | Known admin/service acct |
| Asset Criticality | Crown Jewel / Tier-1 | Standard workstation | Test/dev/scanner |
| Behavioral Baseline | No prior history | Occasional activity | Recurring same time |
| IOC Enrichment | 2+ TI hits | Unknown/grey | Whitelisted/trusted CDN |
| Attack Chain | Correlated alerts, kill chain fit | Isolated alert | Completely isolated, no follow-on |

**Score Interpretation:**
- **+3 to +5** → **TRUE POSITIVE — Escalate to L2 immediately**
- **+1 to +2** → **LIKELY TP — Escalate with Medium priority, document reasoning**
- **0** → **AMBIGUOUS — Gather more evidence before closing; consider brief L2 consult**
- **-1 to -2** → **LIKELY FP — Close with full documentation and tuning recommendation**
- **-3 to -5** → **CONFIRMED FP — Close, document, recommend rule tuning**

---

## PHASE 4 — TRUE POSITIVE ESCALATION PACKAGE

When escalating to L2, always include:

```
TP ESCALATION REPORT (5W FORMAT)
══════════════════════════════════
WHO:     [User account | Asset | IP involved]
WHAT:    [Alert name | Technique (MITRE ATT&CK TID) | Behavior observed]
WHEN:    [Timestamp of first event | Duration | Timeline of correlated events]
WHERE:   [Source host | Destination | Network segment]
WHY:     [Evidence summary — why this is TP, IOC hits, behavioral anomalies]

CONFIDENCE SCORE:  [X/5]
KILL CHAIN STAGE:  [Recon | Delivery | Exploit | Install | C2 | Exfil | Impact]
MITRE ATT&CK:      [Tactic: ___  | Technique: T____]
RECOMMENDED ACTION: [Isolate host | Block IP | Disable account | Preserve evidence | None yet]
ARTIFACTS:         [Log snippets | Screenshot | Hash | IOC list]
SLA STATUS:        [Within SLA | At risk — X minutes remaining]
```

> **CRITICAL FOR MSSP:** Always note SLA status. If SLA is at risk, flag it BEFORE completing investigation — alert the L2 lead immediately.

---

## PHASE 5 — FALSE POSITIVE CLOSURE PROTOCOL

Closing an FP is NOT the end. Every FP must:

1. **Document why it is FP** (evidence, not opinion):
   - "Alert fired on scheduled task `BackupExec.exe` running nightly at 02:00 — confirmed benign by change ticket #1234"
   - NOT: "Looks fine, closing as FP"

2. **Identify the root cause:**
   - Overly broad detection rule
   - Whitelisting gap (known-good process not excluded)
   - No business context in alert (enrichment gap)
   - Environmental noise (scanner, backup, IT tool)

3. **Recommend tuning action:**
   - Suppress: "Add exception for this specific hash/user/host combo"
   - Tune: "Narrow rule scope to exclude process X with parent Y"
   - Whitelist: "Add IP/domain to known-good list"
   - Escalate rule quality issue to Detection Engineer

4. **FP Closure Note Template:**
   ```
   VERDICT: False Positive
   REASON:  [Specific evidence]
   ROOT CAUSE: [Why rule fired incorrectly]
   TUNING REC: [Action to reduce recurrence]
   REVIEWED BY: [Analyst name / ID]
   ```

---

## PHASE 6 — EDGE CASES & SENIOR ANALYST WISDOM

### The "Benign Explained" Trap
> "I can explain it, so it must be FP."

⚠️ **WRONG.** Attackers deliberately mimic legitimate behavior. An attacker using a whitelisted tool (LOLBin — Living Off the Land Binary) like `certutil.exe` to download malware *can* be explained. Explanation is NOT exoneration. Demand corroborating benign evidence (change ticket, known schedule, IT confirmation).

---

### The "Single Alert" Blindspot
> "It's just one alert, probably nothing."

⚠️ **WRONG.** The most dangerous attacks start as one quiet, explainable alert. Always check: what happened on that host in the 24 hours around this alert. One alert can be the tip of an iceberg.

---

### The "I've Seen This Before" Bias
> "This looks exactly like last week's FP, closing it."

⚠️ **DANGEROUS.** Adversaries reuse TTPs. Last week's pattern could be a live campaign this week. Always re-enrich IOCs. Threat intel changes daily.

---

### The "Service Account = Auto FP" Mistake
> "It's just a service account doing its thing."

⚠️ **WRONG.** Service accounts are a primary target for attackers. A service account showing interactive login, lateral movement, or command execution outside its normal scope is a **major TP signal**. Treat service account anomalies seriously.

---

### The MSSP SLA Time Crunch Trap
> "I need to close this fast to meet SLA."

⚠️ **NEVER sacrifice verdict quality for SLA speed.** If you need more time, mark the alert as "In Progress / Under Investigation" to pause SLA clock (if your platform allows), then investigate properly. A wrong verdict is worse than a late one.

---

## QUICK REFERENCE — ANALYST FIELD GUIDE

### 60-Second Triage (for obvious cases only)

**Obvious FP signals (all must be true):**
- Known whitelisted source (internal scanner, backup agent, trusted admin)
- Confirmed change ticket or business justification exists
- No correlated alerts in last 24h
- IOCs are clean on all TI platforms
- Historical pattern matches exactly

**Obvious TP signals (any one is sufficient):**
- IOC confirmed malicious by 2+ TI feeds
- Active C2 communication detected
- Credential dumping tool (Mimikatz, LaZagne) on non-pentest host
- Ransomware encryption activity detected
- Data exfiltration to unknown foreign IP

---

### Questions to Ask Before Closing ANY Alert

```
1. Can I explain this with 100% confidence AND provide documented evidence?
2. Have I checked IOCs against at least 2 TI sources?
3. Have I looked at the 30 minutes before AND after the alert?
4. Is there ANY correlated activity on this host/user/network?
5. Would I be comfortable defending this verdict to my L2 and the client?
```

If you hesitate on ANY of these — do not close. Consult L2.

---

## VERDICT OUTPUT FORMAT

When providing a triage verdict, always structure the output as:

```
╔══════════════════════════════════════════╗
║         TRIAGE VERDICT REPORT            ║
╠══════════════════════════════════════════╣
║ VERDICT:         [TRUE POSITIVE ⚠️ / FALSE POSITIVE ✅ / AMBIGUOUS 🔍]
║ CONFIDENCE:      [X/5]
║ ALERT TYPE:      [Category]
║ MITRE ATT&CK:    [Tactic | Technique ID]
╠══════════════════════════════════════════╣
║ EVIDENCE SUMMARY
║ TP Signals:      [List of factors pointing to TP]
║ FP Signals:      [List of factors pointing to FP]
╠══════════════════════════════════════════╣
║ NEXT ACTION:     [Escalate L2 / Close FP / Gather more evidence]
║ SLA NOTE:        [If MSSP context — SLA status]
║ TUNING REC:      [If FP — what to tune]
╚══════════════════════════════════════════╝
```

---

## REFERENCE FILES

- `references/playbooks.md` — Detailed per-alert-type playbooks (Phishing, Malware, C2, Brute Force, Lateral Movement, Exfiltration, Privilege Escalation, PowerShell, Cloud Alerts)
- `references/mitre-triage-map.md` — MITRE ATT&CK technique → common FP sources mapping
- `references/ti-tools.md` — Threat Intelligence tools quick reference (free + commercial)
- `references/sla-guide.md` — MSSP SLA management during triage

> Load these references when handling a specific alert type or when the analyst asks for deeper guidance on a technique.
