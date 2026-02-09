# Linux Hardening

This project focuses on strengthening the security posture of a Linux server through configuration hardening, auditing, and logging improvements.

## Objectives
- Apply practical hardening techniques to reduce attack surface.
- Implement auditing and monitoring to detect anomalous activity.
- Document changes and validate improvements

## Contents
- 'hardening-steps.md' - Step-by-step hardening procedures.
- 'before-after-security.md' - Comparison of system posture before and after hardening.
- 'auditd-config/' - Audit rules and configuration files.
- 'screenshots/' - Evidence of configuration and validation steps.

## Hardening Areas
- SSH configuration (Key-based auth, restricted ciphers, rate limiting).
- Firewall configuration used UFW or iptables.
- Files system permissions and ownership review.
- Auditd configuration for system event monitoring.
- Logging improvements and integration with SIEM.

## Key Learning Outcomes
- Practical experience applying secure baselines.
- Understanding of how to validate hardening using tools as Lynis.
- Ability to justify configuration decision using risk-based reasoning.