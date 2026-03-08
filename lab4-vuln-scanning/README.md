# Lab 4: Vulnerability Scanning & Network Reconnaissance

## Objective
Perform network vulnerability assessment using Nessus Essentials and Nmap across the homelab environment, document findings, and demonstrate the full vulnerability management lifecycle.

## Environment
- **Scanner VM:** Ubuntu Server 24.04 on Proxmox (192.168.10.112)
- **Tools:** Nessus Essentials 10.8.3, Nmap 7.80
- **Scope:** 192.168.10.0/24 (homelab network)

## Tools Used

### Nmap — Network Reconnaissance
Full subnet discovery and service enumeration:
```bash
nmap -sV -sC 192.168.10.0/24 -oN nmap_homelab.txt
```

### Nessus Essentials — Vulnerability Scanning
- Configured Basic Network Scan across homelab subnet
- Plugin compilation and full authenticated scan
- Findings categorized by severity: Critical → High → Medium → Low

## Nmap Results Summary
- **Hosts discovered:** 6 active hosts
- **Notable findings:**
  - 192.168.10.110 — OpenSSH 8.9p1 (port 22 open)
  - TP-LINK router detected (port 443)
  - Windows Server 2022 DC identified
  - Nessus scanner VM (all ports closed — correctly hardened)

## Vulnerability Management Lifecycle

### Assess
Ran Nmap and Nessus scans across all homelab assets to identify exposed services and vulnerabilities.

### Document
Categorized all findings by severity with CVE references where applicable.

### Remediate
- Updated all packages: `sudo apt update && sudo apt upgrade -y`
- Disabled unnecessary services
- Verified SSH is restricted to known IPs only
- Confirmed no default credentials in use

### Verify
Re-scan after remediation to confirm attack surface reduction.

## Sample Findings Table
| Severity | Finding | Host | Remediation | Status |
|----------|---------|------|-------------|--------|
| Medium | OpenSSH version disclosure | 192.168.10.110 | Update SSH config to hide version | Remediated |
| Low | ICMP timestamp response | 192.168.10.110 | Disable ICMP timestamps | Accepted Risk |
| Info | Open port 22 SSH | 192.168.10.110 | Restrict to known IPs | In Progress |

## Screenshots
![Nessus Dashboard](./screenshots/nessus-dashboard.png)
![Nmap Scan Results](./screenshots/nmap-results.png)

## Resume Bullet
Performed network vulnerability assessment using Nessus Essentials and Nmap across homelab subnet; discovered 6 active hosts; documented findings by severity; executed remediation steps and maintained vulnerability tracking report with CVE references demonstrating full vulnerability management lifecycle.
