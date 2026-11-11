# SSH Brute-Force Detection & Incident Response Lab

A defensive security lab for studying SSH password-guessing activity, detecting suspicious authentication patterns, and documenting an incident response. Run the exercise only in an isolated environment that you own or are authorized to use.

## Project Overview

SSH servers commonly record failed and successful login attempts. By reviewing these events together, defenders can identify repeated password guesses and investigate a possible account compromise.

This project demonstrates a simple blue-team workflow: establish a baseline, review authentication logs, apply a Sigma detection rule, investigate suspicious activity, and document containment and recovery.

## Objectives

- Understand where Linux SSH authentication events are recorded.
- Identify repeated failed logins from the same source address.
- Correlate failures with a later successful login from that source.
- Map relevant activity to MITRE ATT&CK.
- Practice documenting investigation findings and response actions.

## Lab Environment

The lab uses Linux systems and OpenSSH in a controlled, isolated setup. The target's authentication logs provide the event data for review. The repository also includes a Sigma rule and an incident report to support the detection and investigation portions of the exercise.

**Tools and references:**

- Ubuntu Linux
- OpenSSH
- Hydra (used only in the authorized lab simulation)
- Fail2Ban
- Sigma
- MITRE ATT&CK

## Investigation Workflow

1. **Establish a baseline:** Review normal SSH authentication activity on the lab target.
2. **Observe the test activity:** Look for repeated failed password attempts and note the source address and timestamps.
3. **Correlate events:** Check whether a successful login from the same source follows the failures within a short period.
4. **Validate the alert:** Compare the observed events with `sigma-rule.yml`. Confirm that the fields and log format match the target system.
5. **Investigate the account:** Determine which account was accessed and review relevant activity after the login.
6. **Document and respond:** Record evidence, assess impact, contain the activity, and recommend recovery steps.

## Detection Logic

The detection idea is a sequence of multiple failed SSH authentications followed by a successful authentication from the same source within a defined time window. This may indicate password guessing that resulted in access.

Treat the pattern as an investigation lead, not proof of compromise. Legitimate users can mistype passwords, and NAT, VPNs, or shared jump hosts can make many users appear to come from one IP address. Validate the account, timing, source, and surrounding activity before taking action.

## MITRE ATT&CK Mapping

- **T1110 — Brute Force:** Repeated attempts to gain access by guessing credentials.
- **T1110.001 — Password Guessing:** Guessing passwords for an account or service.
- **T1078 — Valid Accounts:** Potential use of valid credentials after a successful login.

## Incident Response Guide

If the evidence suggests unauthorized access:

1. Preserve relevant authentication logs and record timestamps, source addresses, and affected accounts.
2. Confirm whether the successful login was expected with the account owner or system administrator.
3. Contain the risk using your organization's approved process, such as restricting the source or disabling a compromised account.
4. Rotate affected credentials and review SSH keys and account access.
5. Inspect activity after the login and determine whether other systems or accounts may be affected.
6. Harden SSH access, monitor for recurrence, and document the actions and findings.

For prevention, prefer key-based authentication where appropriate, restrict SSH access to trusted networks, use Fail2Ban or an equivalent control, and alert on unusual authentication activity. Follow local policies before changing access or blocking addresses.

## Repository Contents

- `lab-setup.md` — lab environment setup and exercise steps.
- `sigma-rule.yml` — detection rule for the SSH authentication pattern.
- `incident-report.md` — SOC-style investigation summary.
- `README.md` — project overview and workflow.

## Notes and Limitations

Log formats and field names vary across Linux distributions and collection tools. The Sigma rule may need adjustments for your log source and backend. This lab is for defensive learning in an authorized environment; it is not a production-ready monitoring or incident response system.
