# Windows Active Directory Security

This project demonstrates the deployment and hardening of a Windows Acitve Directory environment, including Group Policy configuration and event logging.

## Objectives
- Build a functional AD domain with security-focused configuration.
- Apply Group Policy Objects (GPOs) to enforce security baselines.
- Implement logging and monitoring using Sysmon and SIEM integration.

## Contents
- 'ad-architecture.png' - Diagram of the AD environment.
- 'gpo-baseline.md' - Documented security GPOs and rationale.
- 'sysmon-config.xml' - Sysmon configuration for detailed event logging.
- 'event-logging-pipeline.md' - Steps for forwarding logs to SIEM.

## Security Controls Implemented
- Account lockout policies
- Credential protection and restricted admin mode.
- Application whitelisting (where applicable)
- Enhanced logging via Sysmon and Windows Event Forwarding.
- Hardened domain controller configuration.

## Key Learning Outcomes
- Understanding of AD security fundamentals.
- Ability to configure and justify GPOs.
- Experience building a monitoring pipeline for Windows environments.