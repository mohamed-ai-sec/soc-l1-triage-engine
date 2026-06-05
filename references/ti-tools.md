# Threat Intelligence Tools — Quick Reference
## TRIAGE MASTER v1.0.0 | Reference File

---

## FREE TOOLS (Use for every investigation)

| Tool | URL | Use For |
|---|---|---|
| VirusTotal | virustotal.com | Hash, IP, Domain, URL reputation |
| AbuseIPDB | abuseipdb.com | IP abuse history, attack type |
| AlienVault OTX | otx.alienvault.com | Full IOC enrichment, pulse search |
| Shodan | shodan.io | What is running on that IP? |
| URLScan.io | urlscan.io | URL / domain analysis with screenshot |
| MXToolbox | mxtoolbox.com | SPF/DKIM/DMARC, email header analysis |
| Talos Intelligence | talosintelligence.com | Cisco Talos IP/domain reputation |
| IBM X-Force | exchange.xforce.ibmcloud.com | IP/URL/hash reputation |
| ThreatFox | threatfox.abuse.ch | Malware IOC database |
| Malware Bazaar | bazaar.abuse.ch | Malware hash database |
| URLhaus | urlhaus.abuse.ch | Malicious URLs database |
| Any.run | any.run | Interactive malware sandbox (free tier) |
| Hybrid Analysis | hybrid-analysis.com | Malware sandbox analysis |
| WHOIS | whois.domaintools.com | Domain registration date, registrant |
| BGP Toolkit | bgp.he.net | ASN lookup (CDN vs bulletproof hosting) |

---

## QUICK ENRICHMENT WORKFLOW

### For an IP address:
1. AbuseIPDB → Abuse score + attack type
2. VirusTotal → Detection ratio
3. Shodan → What services are running
4. BGP Toolkit → Is it a CDN? (Akamai, Cloudflare, AWS = likely FP)

### For a domain:
1. VirusTotal → Detection ratio + category
2. URLScan.io → Screenshot + redirect chain
3. WHOIS → Registration date (< 30 days = HIGH RISK)
4. AlienVault OTX → Known threat associations

### For a file hash:
1. VirusTotal → Detection count (0-1 = likely FP or unknown; 5+ = likely TP)
2. Malware Bazaar → Known malware family
3. Hybrid Analysis → Behavioral sandbox report

### For an email header:
1. MXToolbox Header Analyzer → Paste full header
2. Check SPF / DKIM / DMARC result
3. Check sending IP against AbuseIPDB
4. Check sender domain registration date

---

## VERDICT THRESHOLDS (VT Detections)

| VT Detection Count | Interpretation |
|---|---|
| 0 | Unknown — check sandbox behavior |
| 1-2 | Likely heuristic/FP — verify with sandbox |
| 3-5 | Suspicious — treat as medium confidence TP |
| 6-10 | High confidence TP |
| 10+ | Confirmed malicious |

> Note: Some malicious tools (custom RATs, targeted malware) have 0 VT detections. Zero detections ≠ clean. Check behavioral sandbox too.

---

## ASN QUICK REFERENCE — Common CDNs (Usually FP)

- AS13335 — Cloudflare
- AS16509 — Amazon AWS
- AS15169 — Google / GCP
- AS8075 — Microsoft Azure
- AS20940 — Akamai
- AS54113 — Fastly

> If destination IP is in one of these ASNs, it is almost certainly a FP (legitimate SaaS). Confirm by checking which SaaS service the business uses.
