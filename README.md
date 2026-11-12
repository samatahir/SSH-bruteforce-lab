# SSH Brute-Force Detection & Incident Response 

[![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)](https://github.com/samatahir/SSH-bruteforce-lab)
[![Type](https://img.shields.io/badge/Type-Blue%20Team%20Lab-blue?style=for-the-badge)](https://github.com/samatahir/SSH-bruteforce-lab)
[![Platform](https://img.shields.io/badge/Platform-Ubuntu%20Linux-orange?style=for-the-badge)](https://github.com/samatahir/SSH-bruteforce-lab)

This project is for learning how to detect repeated SSH login failures, investigate and document an incident response.
## Overview

Linux SSH services record authentication events that can help defenders identify password-guessing activity. This project demonstrates a blue-team workflow: establish a baseline, review authentication logs, validate a detection rule, investigate suspicious activity, and recommend containment and recovery steps.

## Objectives

- Find SSH authentication events in Linux logs.
- Identify repeated failures from one source address.
- Check for a later successful login from that source.
- Map the activity to MITRE ATT&CK
- Document investigation and response actions.

## Lab Environment and Tools

We are using Ubuntu/Linux systems with OpenSSH in a controlled, isolated setup. Reviewing the target's authentication logs, `/var/log/auth.log` on Ubuntu. 

- Ubuntu Linux and OpenSSH
- Hydra for authorized lab simulation only
- Fail2Ban for defensive blocking
- Sigma for expressing detection logic
- MITRE ATT&CK for technique mapping


## Investigation Workflow

1. **Establish a baseline:** Review normal SSH authentication activity on the target.
2. **Review events:** Note repeated failed logins, timestamps, account names, and source addresses.
3. **Correlate activity:** Check whether a successful login from the same source follows within a short time window.
4. **Validate detection:** Compare the events with [`sigma-rule.yml`](sigma-rule.yml), and confirm its fields match your log source and detection backend.
5. **Investigate:** Verify whether the login was expected and review activity associated with the account.
6. **Document:** Record evidence, assessment, actions taken, and follow-up recommendations in [`incident-report.md`](incident-report.md).

## Detection Logic

Here the detection looks for multiple failed SSH authentications followed by a successful authentication from the same source within a defined period. This pattern might indicate password guessing that resulted in access.

Alert is only an investigation lead, not proof of compromise. Mistyped passwords or a number of things can produce failures, while NAT, VPNs, or shared jump hosts can make multiple users appear to share one address. It may or may not be the case. Check the account, timing, surrounding activity before drawing conclusions.

## MITRE ATT&CK Mapping

- **T1110 — Brute Force:** Repeated attempts to gain access by guessing credentials.
- **T1110.001 — Password Guessing:** Guessing passwords for an account or service.
- **T1078 — Valid Accounts:** Might apply if valid credentials were used to gain access.

## Incident Response

If investigation suggests unauthorized access:

1. Preserve relevant logs and record timestamps, source addresses, and affected accounts.
2. Confirm whether the login was expected with the account owner or system administrator.
3. Contain the risk using your approved process, aka restricting the source or disabling a compromised account.
4. Rotate affected credentials and review SSH keys and account permissions.
5. Inspect activity after the login and assess whether other systems or accounts were affected.
6. Harden SSH access, monitor for recurrence, and document findings and recovery actions.

For prevention, use key-based authentication, restricting SSH to trusted networks, Fail2Ban or an equivalent control, and alerts for unusual authentication activity. Follow policies before changing access or blocking addresses.

## Repository Contents

- [`lab-setup.md`](lab-setup.md) — lab environment and exercise instructions.
- [`sigma-rule.yml`](sigma-rule.yml) — detection rule for the SSH authentication pattern.
- [`incident-report.md`](incident-report.md) — investigation summary.

## Limitations
 Sigma rule might need adjustments because log formats vary across systems. 
