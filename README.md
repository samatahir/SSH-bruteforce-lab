# SSH Brute-Force Detection & Incident Response

[![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)](https://github.com/samatahir/SSH-bruteforce-lab)
[![Type](https://img.shields.io/badge/Type-Blue%20Team%20Lab-blue?style=for-the-badge)](https://github.com/samatahir/SSH-bruteforce-lab)
[![Platform](https://img.shields.io/badge/Platform-Ubuntu%20Linux-orange?style=for-the-badge)](https://github.com/samatahir/SSH-bruteforce-lab)

A controlled, isolated home lab built to simulate an SSH brute-force attack, validate detection logic, and document an incident response workflow.

## Overview

This repository contains a practical security exercise focused on:

- SSH authentication abuse detection
- Linux authentication log analysis
- MITRE ATT&CK mapping
- Sigma rule creation
- SOC-style incident documentation

The lab was designed to demonstrate how malicious login attempts can be identified from system logs and how defenders can respond before a compromise becomes a full incident.

## Lab Objective

The goal is to recreate a realistic attack path in a contained environment:

1. Establish a normal Linux baseline
2. Perform a simulated brute-force attack against SSH
3. Observe repeated failures in `/var/log/auth.log`
4. Detect the successful login that follows
5. Correlate events to a likely credential compromise
6. Apply containment and remediation controls

## Architecture

- Attacker host: Ubuntu/Linux system running Hydra
- Target host: Ubuntu/Linux SSH server
- Monitoring source: `auth.log` and authentication telemetry
- Detection: Sigma rule for brute-force + successful login pattern

## Tools Used

- Ubuntu Linux
- OpenSSH
- Hydra
- Fail2Ban
- Sigma
- MITRE ATT&CK framework

## Attack Flow

| Time | Event |
| --- | --- |
| T+00:00 | Baseline authentication activity established |
| T+00:05 | Failed SSH password attempts begin from a single source IP |
| T+00:07 | SSH login succeeds using a guessed credential |
| T+00:10 | Log correlation confirms compromise |
| T+00:15 | Containment and remediation actions start |

## Detection Logic

The core pattern being monitored is:

- multiple failed SSH login attempts from one source IP
- followed by a successful SSH authentication from the same IP within a short window

This is a classic brute-force-to-success pattern often associated with credential access and password guessing.

## MITRE ATT&CK Mapping

| Technique | ID | Description |
| --- | --- | --- |
| Brute Force | T1110 | Automated password guessing or credential stuffing |
| Password Guessing | T1110.001 | Repeated attempts against a known service |
| Valid Accounts | T1078 | Successful use of compromised credentials |

## Repository Contents

- `README.md` — project overview and attack flow
- `incident-report.md` — SOC-style investigation summary
- `sigma-rule.yml` — Sigma detection rule for the SSH brute-force pattern
- `lab-setup.md` — environment setup and reproduction steps

## Suggested Remediation

- Disable password-based SSH access in favor of key-based authentication
- Use Fail2Ban or similar automation to block repeated failures
- Restrict SSH access to trusted IP ranges or jump hosts
- Enable real-time alerting on authentication anomalies
- Require MFA or stronger access controls for privileged accounts

## Responsible Use

This lab is intended for authorized, isolated, and learning-focused environments only. Do not run brute-force testing against systems you do not own or have explicit permission to evaluate.

## Related Files

- `sigma-rule.yml`
- `incident-report.md`
- `lab-setup.md`

