# 🛡️ Weekend Lab Marathon — Cybersecurity Homelab Portfolio

> Built in a single weekend on a Proxmox homelab. Every tool is real, every detection fired, every skill is documented.

## About
Entry-level cybersecurity portfolio demonstrating hands-on SOC Analyst and Security Engineer skills built across 4 production-quality labs in 48 hours.

**Environment:** Proxmox hypervisor | Ubuntu Server VMs | Windows Server 2022 | AWS

---

## Labs Completed

| # | Lab | Tools | Role Focus |
|---|-----|-------|------------|
| 1 | [Splunk SIEM & Detection Engineering](./lab1-splunk-siem/) | Splunk Enterprise, Linux auth logs | SOC Analyst |
| 2 | [Active Directory Attack & Detection](./lab2-active-directory/) | Windows Server 2022, AD DS, PowerShell, Splunk Universal Forwarder | SOC Analyst |
| 3 | [AWS Cloud Security](./lab3-aws-cloud/) | IAM, CloudTrail, CloudWatch, AWS Config | Security Engineer |
| 4 | [Vulnerability Scanning & Recon](./lab4-vuln-scanning/) | Nessus Essentials, Nmap | Both |

---

## Key Skills Demonstrated
- SIEM deployment and SPL detection query authoring (Splunk)
- Active Directory administration — OUs, security groups, GPOs
- Identity lifecycle management — Joiner/Mover/Leaver via PowerShell bulk scripting
- Attack simulation — brute force (Event ID 4625), privilege escalation recon
- Cloud security posture — AWS IAM least privilege, CloudTrail audit logging, CloudWatch alerting
- Network vulnerability assessment — Nessus scanning, Nmap reconnaissance, remediation documentation

---

## Lab 2 Highlight — NBA-Themed AD Environment
Built a creative enterprise AD simulation using NBA teams as departments:
- **4 teams as OUs:** Celtics | Rockets | Thunder | Lakers
- **40 real players** bulk-onboarded via PowerShell script
- **Coaches** = privileged accounts with elevated group memberships
- **Lifecycle simulation:** KD trade (Mover) + Maxi Kleber released (Leaver)
- **Attack simulation:** 10x brute force against ljames account → detected via Event ID 4625

---

## Certifications
- CompTIA Security+ (February 2026)
- ISC² CC

## Connect
- GitHub: [coreyarmstrong-sec](https://github.com/coreyarmstrong-sec)
- LinkedIn: [your-linkedin-here]
