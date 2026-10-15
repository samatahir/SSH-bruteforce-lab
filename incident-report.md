# Incident Report — SSH Brute Force Attack

**Incident ID:** IR-2026-001  
**Date:** July 28 2026    
**Severity:** High  
**Status:** Resolved  

---

## Executive Summary

A brute-force SSH attack was detected against a Linux target host.
The attacker used Hydra to systematically attempt credentials until
a successful authentication was achieved, confirming credential compromise.

---

## Indicators of Compromise (IOCs)

| IOC Type | Value | Description |
|---|---|---|
| Source IP | 192.168.x.x | Attacking machine |
| Username | [target user] | Compromised account |
| Tool | Hydra | Brute-force tool |
| Auth Method | Password | SSH password auth |

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---|---|---|
| Credential Access | Brute Force | T1110 |
| Credential Access | Password Guessing | T1110.001 |
| Persistence | Valid Accounts | T1078 |

---

## Root Cause

Password-based SSH authentication was enabled with no rate limiting
or lockout policy, allowing unlimited credential attempts.

---

## Remediation

**Immediate:**
- Block attacking IP at firewall
- Force password reset on compromised account
- Review commands executed post-compromise

**Short-term:**
- Deploy Fail2Ban — auto-block after 5 failed attempts
- Disable password auth, enforce SSH keys only

**Long-term:**
- Real-time alerting on brute-force patterns
- Restrict SSH to known IP allowlist
- Enable MFA for all remote access

---

## Lessons Learned

Key-based authentication combined with Fail2Ban would have
prevented this attack entirely. Password-based SSH with no
lockout policy is a critical misconfiguration in any environment.
