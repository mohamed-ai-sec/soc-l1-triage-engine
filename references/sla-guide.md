# MSSP SLA Management During Triage
## TRIAGE MASTER v1.0.0 | Reference File

---

## WHY SLA MATTERS IN MSSP TRIAGE

In an MSSP environment, your verdict is not just a technical decision — it is a **contractual and commercial obligation**. Every alert has an SLA clock. Missing it has real consequences:

- Financial penalties per SLA breach clause
- Client escalation to account management
- Contract termination risk for repeated breaches
- Reputational damage — clients talk to each other
- Regulatory exposure if client operates in a regulated sector (PCI, HIPAA, ISO 27001)

> **Senior SOC wisdom:** An accurate verdict delivered 5 minutes late is better than a wrong verdict delivered on time. But a wrong verdict delivered on time is the worst outcome of all.

---

## STANDARD MSSP SLA TIERS

Most MSSPs operate on these approximate SLA windows (verify your specific client SLA):

| Severity | Acknowledge By | Triage/Verdict By | Escalation By |
|---|---|---|---|
| **Critical (P1)** | 15 minutes | 30 minutes | 30 minutes |
| **High (P2)** | 30 minutes | 1 hour | 1 hour |
| **Medium (P3)** | 1 hour | 4 hours | 4 hours |
| **Low (P4)** | 4 hours | 24 hours | N/A |

> These are common benchmarks. **Always check the specific client's SLA document.** Some financial/healthcare clients have P1 SLAs as tight as 10 minutes.

---

## SLA CLOCK MANAGEMENT

### What starts the SLA clock?
- Alert creation timestamp in SIEM/ticketing system
- NOT when you pick it up — from the moment it was created

### How to check SLA status
1. Open the alert in your SIEM/ticketing system
2. Find the "SLA Due" or "Breach Time" field
3. Calculate: `SLA Due Time - Current Time = Remaining`

### SLA Status Flags (use in every TP escalation)
```
🟢 SLA SAFE    — More than 30% of SLA window remaining
🟡 SLA AT RISK — Less than 30% of SLA window remaining  
🔴 SLA BREACH  — Less than 10 minutes remaining
💀 SLA MISSED  — Already breached
```

---

## THE SLA TRIAGE PROTOCOL

### When SLA is SAFE (>30% remaining)
→ Conduct full 5-Dimension investigation
→ Complete all enrichment steps
→ Deliver accurate verdict with full documentation

### When SLA is AT RISK (<30% remaining)
→ Prioritize the 2 most decisive dimensions:
   1. IOC Enrichment (VirusTotal + AbuseIPDB — 2 minutes)
   2. Behavioral Baseline (30-min before/after — 3 minutes)
→ Use Confidence Score model to reach best-available verdict
→ **Immediately notify L2 lead** that SLA is at risk
→ Add note: "Preliminary verdict — SLA constraint — full analysis pending"

### When SLA is CRITICAL (<10 min remaining)
→ Use the 60-Second Obvious FP/TP signals ONLY
→ Make best-available verdict NOW
→ Escalate to L2 with full context: "SLA breach imminent — preliminary verdict — needs review"
→ Document that SLA was at risk and why (workload, alert queue surge, etc.)
→ NEVER close a critical/high alert as FP under SLA pressure without minimum IOC check

### When SLA is MISSED
→ Still complete the investigation — do not skip steps
→ Flag as SLA breach in ticketing system
→ Add breach reason note: specific cause (queue surge, missing enrichment data, tool outage)
→ Notify your team lead immediately
→ Do not attempt to hide or backdate the breach

---

## PRIORITY QUEUE MANAGEMENT

When multiple alerts are active simultaneously, triage in this order:

### Step 1: Sort by severity first
Critical → High → Medium → Low

### Step 2: Within same severity, sort by SLA remaining
Least time remaining = handle first

### Step 3: Within same severity AND similar SLA, sort by asset criticality
Crown Jewel / Tier-1 assets → first

### Step 4: Watch for correlation
Before diving into each alert individually, scan for alerts sharing the same:
- Source IP or hostname
- User account
- Destination

Multiple alerts on same entity = **handle as cluster, not individual alerts** = faster + higher accuracy

---

## CLIENT COMMUNICATION STANDARDS

### When to notify the client directly
- **TP Confirmed (Critical/High):** Your L2 / SOC Lead notifies client — NOT L1 alone
- **Major FP Storm:** Alert L2 lead to consider notifying client about detection rule noise
- **SLA Breach:** Your team lead handles client communication

### What L1 NEVER does alone
- Does NOT independently contact the client
- Does NOT escalate directly to the client CISO
- Does NOT make containment decisions (isolate, block, disable) without L2 authorization
  - **EXCEPTION:** If your runbook explicitly authorizes L1 to perform specific containment actions (e.g., host isolation via EDR), then follow the runbook
- Does NOT modify SIEM detection rules

---

## MULTI-CLIENT CONTEXT (MSSP-SPECIFIC)

Working in a multi-tenant MSSP environment means:

### 1. Client baseline is client-specific
A "suspicious" process on Client A may be the core business tool of Client B.
Always check the client profile in your CMDB/client notes before rendering a verdict.

### 2. Approved tool lists are client-specific
- Client A approved: AnyDesk, PsExec (IT use)
- Client B: AnyDesk is explicitly prohibited (high-risk sector)
Same alert → different verdict → because different client context.

### 3. Regulated clients have higher bar
For clients in Finance (PCI DSS), Healthcare (HIPAA), Critical Infrastructure:
- Raise your TP threshold (when in doubt → escalate rather than close)
- SLA breaches carry higher consequences
- Evidence preservation is more critical

### 4. Pentest / Red Team windows
Some clients run regular red team exercises. ALWAYS check for active engagement windows before calling a TP. However:
> **Rule:** If you do not have CONFIRMED written authorization that a pentest is active RIGHT NOW → treat it as TP.
> Never assume a pentest is running based on the "feel" of the alert.

---

## SLA BREACH POST-MORTEM TEMPLATE

For every SLA breach, complete this within 24 hours:

```
SLA BREACH REPORT
═════════════════════════════════
Alert ID:         [ID]
Client:           [Client name]
Severity:         [P1/P2/P3]
SLA Window:       [e.g., 30 minutes]
Actual Handle Time: [e.g., 47 minutes]
Breach Duration:  [17 minutes over]

ROOT CAUSE:
[ ] Alert queue surge (volume)
[ ] Tool unavailability (SIEM/TI platform down)
[ ] Missing client context (CMDB not updated)
[ ] Analyst knowledge gap
[ ] Escalation delay (L2 unavailable)
[ ] Other: _______________

IMPACT:
[ ] No SLA penalty clause triggered
[ ] Penalty clause triggered — amount: ___
[ ] Client notified

CORRECTIVE ACTION:
[What will be done to prevent recurrence]

Analyst:          [Name/ID]
Reviewed by:      [Team Lead]
```

---

*This file is critical for MSSP operations. Review it at the start of every shift. Know your clients' SLA windows before you open the first alert.*
