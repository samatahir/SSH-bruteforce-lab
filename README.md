# SSH Brute-Force Detection & Incident Response

**Tools:** Ubuntu · Hydra · Fail2Ban · Sigma · MITRE ATT&CK  
**Type:** Blue Team Home Lab  
**Status:** ![Complete](https://img.shields.io/badge/Complete-brightgreen?style=flat-square) |

---

## Overview

Built an isolated two-machine Linux lab to simulate a real-world SSH 
brute-force attack and conducted an end-to-end SOC investigation.

---

## What I Did

- Set up an isolated attacker and target Linux environment
- Established a baseline of normal authentication activity
- Executed a controlled SSH brute-force attack using Hydra
- Analyzed `/var/log/auth.log` to identify failed and successful logins
- Identified the attacking source IP and confirmed credential compromise
- Mapped findings to MITRE ATT&CK: T1110 and T1078
- Wrote a custom Sigma detection rule
- Documented the full investigation in a SOC-style incident report

---

## Attack Timeline

| Time | Event |
|---|---|
| T+00:00 | Baseline normal authentication established |
| T+00:05 | Burst of failed SSH login attempts detected |
| T+00:07 | Successful SSH authentication from same source IP |
| T+00:10 | Credential compromise confirmed via log correlation |
| T+00:15 | Containment actions initiated |

---

## MITRE ATT&CK Mapping

| Technique | ID | Description |
|---|---|---|
| Brute Force | T1110 | Automated password guessing via Hydra |
| Valid Accounts | T1078 | Successful auth using compromised credential |

---

## Detection Logic

Looked for: multiple failed SSH logins from one source IP 
immediately followed by a successful login — classic brute-force-then-success pattern.

**Sigma Rule:** See `sigma-rule.yml`  
**Incident Report:** See `incident-report.md`

---

## Remediation Recommendations

- Enforce SSH key-based authentication, disable password auth
- Deploy Fail2Ban to auto-block IPs after failed attempts
- Implement real-time alerting on auth.log patterns
- Restrict SSH access to known IP ranges
