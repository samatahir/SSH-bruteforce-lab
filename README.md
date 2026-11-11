# SSH Brute-Force Lab

This lab shows how a brute-force SSH attack looks in a controlled environment and how to detect it.

## Goal

- Test a normal Linux setup
- Simulate repeated SSH login attempts
- Watch authentication logs
- Detect the attack pattern
- Respond to the compromise

## Tools Used

- Ubuntu Linux
- OpenSSH
- Hydra
- Fail2Ban
- Sigma

## Attack Flow

1. A normal SSH login pattern is established.
2. Many failed login attempts come from one IP address.
3. A guessed password eventually succeeds.
4. The logs confirm that the account was compromised.
5. The incident is contained and remediated.

## Detection Logic

The system should alert when:

- multiple failed SSH logins happen from the same source IP
- a successful SSH login happens from that same IP shortly after

This is a classic password-guessing attack.

## MITRE ATT&CK Mapping

- T1110: Brute Force
- T1110.001: Password Guessing
- T1078: Valid Accounts

## Repository Contents

- `README.md` — project summary
- `lab-setup.md` — setup and reproduction steps
- `sigma-rule.yml` — Sigma detection rule
- `incident-report.md` — investigation write-up

## Recommended Response

- Disable password SSH logins
- Block repeated failed attempts with Fail2Ban
- Restrict SSH to trusted IP ranges
- Alert on suspicious authentication activity
- Require stronger authentication for privileged accounts
