# Lab 2: Active Directory Attack & Detection Lab

## Objective
Build an enterprise-grade Active Directory environment using NBA teams as departments, simulate real attacks, and detect them using Windows Event IDs.

## Environment
- **DC:** Windows Server 2022 on Proxmox (NBALAB.local)
- **Tools:** AD DS, PowerShell, Splunk Universal Forwarder
- **Domain:** NBALAB.local

## NBA-Themed AD Architecture
| OU (Department) | Users (Players) | Privileged Accounts (Coaches) |
|-----------------|-----------------|-------------------------------|
| Celtics | 10 players | coach_joemazzulla |
| Rockets | 10 players | coach_imeudoka |
| Thunder | 10 players | coach_markdaigneault |
| Lakers | 10 players | coach_jjredick |

## Security Groups
- GRP_Celtics_Players, GRP_Rockets_Players, GRP_Thunder_Players, GRP_Lakers_Players
- GRP_HeadCoaches — elevated privileges, VPN access, write permissions
- GRP_AllPlayers — base access for all 40 players
- GRP_FilesShare_Read / GRP_FilesShare_Write
- GRP_VPN_Access

## Bulk Onboarding Script
40 players onboarded via PowerShell automation simulating real enterprise IAM:
- Auto-assigned to team OUs and security groups
- Passwords set with forced change on first login
- See: [NBA_AD_Setup.ps1](./NBA_AD_Setup.ps1)

## Identity Lifecycle Simulation

**MOVER — KD Trade (Rockets → Thunder):**
- Moved OU from Rockets to Thunder
- Removed from GRP_Rockets_Players
- Added to GRP_Thunder_Players
- Department attribute updated

**LEAVER — Maxi Kleber Released:**
- Account disabled
- Removed from all security groups
- Moved to Disabled OU
- Description updated with offboarding date

## Attack Simulations & Detections

### Brute Force — Event ID 4625
Simulated 10 failed login attempts against ljames (LeBron James) account.

**Splunk Detection:**
```spl
