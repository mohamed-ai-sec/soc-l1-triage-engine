<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,40:0a1628,100:e94560&height=200&section=header&text=SOC%20L1%20Triage%20Engine&fontSize=46&fontColor=ffffff&fontAlignY=36&fontAlign=50&desc=Elite%20TP%2FFP%20Verdict%20Engine%20%E2%80%94%20MSSP-Grade%20Alert%20Triage%20for%20Claude.ai&descAlignY=58&descSize=15&descColor=a0c4ff&animation=fadeIn" width="100%"/>

<br/>

<a href="https://github.com/mohamed-ai-sec/soc-l1-triage-engine/blob/main/soc-l1-triage-engine.skill"><img src="https://img.shields.io/badge/SKILL.md-Drop--In%20Ready-e94560?style=flat-square&logo=files&logoColor=white" height="24"/></a>&nbsp;
<a href="https://agentskills.io"><img src="https://img.shields.io/badge/agentskills.io-Compliant-238636?style=flat-square" height="24"/></a>&nbsp;
<img src="https://img.shields.io/badge/Claude.ai-Compatible-a0c4ff?style=flat-square&logo=anthropic&logoColor=white" height="24"/>&nbsp;
<img src="https://img.shields.io/badge/Version-1.0.0-0f3460?style=flat-square" height="24"/>&nbsp;
<a href="https://github.com/mohamed-ai-sec/soc-l1-triage-engine/blob/main/LICENSE"><img src="https://img.shields.io/badge/License-MIT-238636?style=flat-square" height="24"/></a>

<br/><br/>

<a href="https://www.linkedin.com/in/mohamedbenbouazza/"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white" height="24"/></a>&nbsp;
<a href="https://github.com/mohamed-ai-sec"><img src="https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white" height="24"/></a>&nbsp;
<a href="https://substack.com/@benbouazza"><img src="https://img.shields.io/badge/Substack-FF6719?style=flat-square&logo=substack&logoColor=white" height="24"/></a>&nbsp;
<a href="https://x.com/benbouazza_m"><img src="https://img.shields.io/badge/X-000000?style=flat-square&logo=x&logoColor=white" height="24"/></a>

</div>

<br/>

---

## `~/what-it-does`

<br/>

```
 ╔══════════════════════════════════════════════════════════════════════════╗
 ║   🛡️  SOC L1 TRIAGE ENGINE  ·  v1.0.0                                  ║
 ║                                                                          ║
 ║   Paste any alert, SIEM event, or EDR detection                         ║
 ║   → Structured TP / FP verdict in seconds                               ║
 ║                                                                          ║
 ║   5-Dimension Context Engine · MITRE ATT&CK mapped                      ║
 ║   Built for MSSP SLA compliance and client trust                         ║
 ╚══════════════════════════════════════════════════════════════════════════╝
```

You paste an alert. The engine asks the right questions, evaluates 5 context dimensions, cross-references per-alert-type playbooks, and returns a **confidence-scored TP/FP verdict** with full escalation documentation — structured the way a senior analyst would think, at the speed of L1 triage.

**Never guess. Never assume. Evidence decides.**

<br/>

---

## `~/what-it-covers`

<br/>

<div align="center">

| Alert Type | Coverage |
|:---|:---:|
| 🎣 Phishing emails | ✅ |
| 🦠 Malware / ransomware execution | ✅ |
| 🔗 C2 beaconing / suspicious outbound | ✅ |
| 🔐 Brute force & credential abuse | ✅ |
| 🔀 Lateral movement | ✅ |
| 📤 Data exfiltration | ✅ |
| ⬆️ Privilege escalation | ✅ |
| 💻 Suspicious PowerShell / scripts | ✅ |
| 👤 Anomalous logins / impossible travel | ✅ |
| ☁️ Cloud audit alerts (AWS / Azure / GCP) | ✅ |
| 🔒 DLP events | ✅ |

</div>

<br/>

---

## `~/how-it-works`

<br/>

```
Alert Input
    │
    ▼
PHASE 0 — Alert Intake
    Extract 10 required fields
    Missing fields? Ask before proceeding
    │
    ▼
PHASE 1 — 5-Dimension Context Engine
    D1: User Context
    D2: Asset Context
    D3: Network/Infrastructure Context
    D4: Behavioral Baseline Context
    D5: Threat Intelligence Context
    │
    ▼
PHASE 2 — Alert-Type Playbook
    Route to one of 7 deep-dive playbooks
    Per-type investigation steps
    Specific FP sources to eliminate
    │
    ▼
PHASE 3 — Confidence Scoring
    TP/FP Score (0–100%)
    Evidence summary
    Escalation recommendation
    │
    ▼
PHASE 4 — Verdict Output
    🟢 FALSE POSITIVE — Close with documentation
    🔴 TRUE POSITIVE  — Escalate with IR package
    🟡 INCONCLUSIVE   — Senior analyst review
```

<br/>

---

## `~/install`

<br/>

**Option 1 — Claude.ai Project (recommended)**

1. Open [Claude.ai](https://claude.ai) → **Projects** → create or open a project
2. Click **Add content** → **Upload file**
3. Upload `soc-l1-triage-engine.skill`
4. The skill activates automatically for every conversation in that project

**Option 2 — Manual paste**

1. Copy the full contents of `SKILL.md`
2. Paste into your Claude.ai project instructions or system prompt

<br/>

> ⚡ **That's it.** No setup. No configuration. No API key. Drop-in and go.

<br/>

---

## `~/repo-structure`

<br/>

```
soc-l1-triage-engine/
│
├── soc-l1-triage-engine.skill   ← Drop-in skill file for Claude.ai Projects
├── SKILL.md                     ← Full skill source (Markdown)
├── README.md                    ← This file
├── LICENSE                      ← MIT
│
└── references/
    ├── playbooks.md             ← 7 per-alert-type deep-dive playbooks
    ├── mitre-triage-map.md      ← 25 MITRE techniques → FP source table
    ├── ti-tools.md              ← 20+ free threat intel tools + cheat sheet
    └── sla-guide.md             ← MSSP SLA management + escalation templates
```

<br/>

---

## `~/output-example`

<br/>

```
═══════════════════════════════════════════════════════════
TRIAGE VERDICT — CASE #[AUTO]
═══════════════════════════════════════════════════════════

ALERT     : Suspicious PowerShell Encoded Command
ANALYST   : L1 Automated Assessment
TIMESTAMP : [extracted from alert]

VERDICT   : 🔴 TRUE POSITIVE
CONFIDENCE: 87%

MITRE ATT&CK:
  • T1059.001 — Command and Scripting Interpreter: PowerShell
  • T1027     — Obfuscated Files or Information

5D CONTEXT SUMMARY:
  D1 User    : Finance user — PowerShell NOT expected for role       [TP signal]
  D2 Asset   : Standard workstation, not admin jump host             [TP signal]
  D3 Network : Command reached out to 185.234.x.x (flagged TI feed) [TP signal]
  D4 Baseline: First occurrence, no historical baseline              [TP signal]
  D5 TI      : IP matches C2 IOC — Emotet campaign (VirusTotal: 47/72) [TP signal]

ESCALATION RECOMMENDED:
  Priority  : HIGH
  Escalate to: L2 within 15 minutes
  Action    : Isolate endpoint immediately

EVIDENCE PACKAGE: [attached to ticket]
═══════════════════════════════════════════════════════════
```

<br/>

---

## `~/reference-files`

<br/>

The `references/` directory ships with four companion documents:

| File | What's inside |
|:---|:---|
| `playbooks.md` | 7 alert-type playbooks — phishing, malware, C2, brute force, lateral movement, exfiltration, privilege escalation. Each includes trigger criteria, investigation steps, FP elimination checklist, and escalation criteria |
| `mitre-triage-map.md` | 25 MITRE ATT&CK techniques mapped to common FP sources and TP differentiators — built for fast technique-to-context lookup during live triage |
| `ti-tools.md` | 20+ free threat intelligence tools organized by use case (IP reputation, domain lookup, file hash, email header analysis) with query cheat sheets |
| `sla-guide.md` | MSSP SLA management — response time thresholds by severity, escalation path templates, client communication templates, SLA breach prevention checklist |

<br/>

---

## `~/built-for`

<br/>

<div align="center">

| WHO | WHY |
|:---|:---|
| 🔵 **L1 SOC Analysts** | Structure your triage — never miss a TP, never drown in FP |
| 🔵 **L2 / L3 Analysts** | Standardize L1 escalations so handoffs are complete and documented |
| 🔵 **MSSP Teams** | SLA compliance built into every verdict — consistent quality across all clients |
| 🔵 **Security Team Leads** | Deploy once across your project — every analyst works at senior level |
| 🔵 **Detection Engineers** | Understand what L1 sees — design better rules, reduce FP noise |

</div>

<br/>

---

## `~/philosophy`

<br/>

```
╔══════════════════════════════════════════════════════════════════════╗
║                                                                      ║
║   AI won't replace experts.                                          ║
║   Experts who know how to use AI will replace those who don't.       ║
║                                                                      ║
║   This skill amplifies analyst judgment — it does not bypass it.     ║
║   The human stays in the loop. The AI eliminates the repetition.     ║
║                                                                      ║
╚══════════════════════════════════════════════════════════════════════╝
```

> Every verdict requires analyst review. The skill provides **structure and speed** — the analyst provides **judgment and accountability**.

<br/>

---

## `~/more-systems`

<br/>

<div align="center">

| SKILL | WHAT IT DOES | STATUS |
|:---|:---|:---:|
| 🚀 **[soc-auto-report-skill](https://github.com/mohamed-ai-sec/soc-auto-report-skill)** | Paste any artifact → 12-section CISO-ready IR report in 30 seconds | ![Live](https://img.shields.io/badge/LIVE-238636?style=flat-square) |
| 🔍 **soc-detection-engineer-skill** | KQL · SPL · SIGMA · YARA — auto-generated detection rules | ![Live](https://img.shields.io/badge/LIVE-238636?style=flat-square) |
| 🛡️ **soc-l1-triage-engine** | TP/FP verdict engine — this repo | ![Live](https://img.shields.io/badge/LIVE-238636?style=flat-square) |
| 🎣 **soc-phishing-skill** | Full phishing analysis pipeline | ![Soon](https://img.shields.io/badge/SHIPPING-0f3460?style=flat-square) |
| 🔎 **soc-threat-hunt-skill** | Threat hunting workflow automation | ![Soon](https://img.shields.io/badge/SHIPPING-0f3460?style=flat-square) |

</div>

<br/>

---
## How it works

![How soc-l1-triage-engine turns an alert into a documented verdict](docs/how-it-works.gif)

| Phase | What happens |
|:---|:---|
| **0 Intake** | Pulls the alert fields (rule, severity, timestamp, source, destination, user, process or hash, raw log, tool, client context). Asks for anything missing. Raw log first. |
| **1 Context** | Scores five dimensions at +1 (TP evidence), 0 or -1 (FP evidence): user context, asset criticality, behavioral baseline, IOC enrichment, attack-chain correlation. |
| **2 Playbook** | Applies the playbook for the alert type: phishing, malware or ransomware, C2 beaconing, brute force, credential abuse, lateral movement, PowerShell, exfiltration, privilege escalation, anomalous login. |
| **3 Verdict** | Decision tree first, then the summed score from -5 to +5. |
| **4-5 Close out** | True positive: 5W escalation package. False positive: closure note with root cause and tuning recommendation. Ambiguous: gather more evidence. |
| **6 Guardrails** | Calls out common mistakes: "I can explain it, so it's FP", "just one alert", "same as last week", "service account, so auto-FP", "close it fast for the SLA". |

| Score | Verdict |
|:---|:---|
| +3 to +5 | True positive. Escalate to L2 immediately |
| +1 to +2 | Likely TP. Escalate, medium priority |
| 0 | Ambiguous. Gather more evidence |
| -1 to -2 | Likely FP. Close with documentation and a tuning rec |
| -3 to -5 | Confirmed FP. Close, document, recommend tuning |

The skill tells you which threat-intel sources to check. You run the lookups and paste the results.

---

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:e94560,40:0a1628,100:0d1117&height=140&section=footer&text=Built%20in%20public.%20Shipped%20free.%20For%20analysts%20who%20think%20in%20systems.&fontSize=14&fontColor=8b949e&fontAlignY=55&reversal=true" width="100%"/>

<div align="center">

**Mohamed Benbouazza** &nbsp;·&nbsp; SOC Systems Architect &nbsp;·&nbsp; [`github.com/mohamed-ai-sec`](https://github.com/mohamed-ai-sec)

⭐ **If this saves your team time, star it.** That is the only metric that matters.

</div>
