# Brute Force / Password Spray Triage Runbook

## Purpose

This runbook is used to investigate repeated failed authentication attempts that may indicate brute force or password spray activity in a Windows and Active Directory environment monitored through Splunk.

## Environment Context

This runbook is based on my lab environment:

- Windows Server 2022 Active Directory domain
- 40+ test users provisioned with PowerShell
- Windows Security logs forwarded into Splunk
- Simulated failed logons and account lockouts
- Detection focus on Event IDs `4625` and `4740`

## Alert Logic

Trigger an alert when one or more of the following conditions are met:

- a single source IP generates repeated failed login attempts against one account
- a single source IP targets multiple accounts in a short time window
- failed login attempts are followed by a successful login
- repeated failures generate account lockout events

Example logic in Splunk:

```spl
index=wineventlog EventCode=4625
| stats count by Account_Name, Source_Network_Address, host
| where count >= 5
| sort - count
```

## Likely Causes

- user typing errors
- stale passwords cached in scripts, scheduled tasks, or mapped drives
- service account authentication failures
- brute force attack against one account
- password spray activity across multiple accounts

## Required Data Sources

- Splunk Windows Security logs
- Event ID `4625` for failed logons
- Event ID `4740` for account lockouts
- username
- source IP address
- destination host
- logon type if available
- time window of activity

## Investigation Steps

1. Identify the affected username, source IP, host, and time range.
2. Determine whether the failed logons are focused on one account or spread across many accounts.
3. Check whether the source IP is internal or external.
4. Review whether a successful login occurred after repeated failures.
5. Search for related account lockout activity using Event ID `4740`.
6. Check whether the affected account is privileged, administrative, or business-critical.
7. Determine whether the activity aligns to a known password reset, service account issue, or user typo pattern.
8. Decide whether the activity is more consistent with brute force, password spray, or a benign misconfiguration.

## Splunk Queries

Failed logons by user and source:

```spl
index=wineventlog EventCode=4625
| stats count by Account_Name, Source_Network_Address, host
| sort - count
```

Lockout review:

```spl
index=wineventlog EventCode=4740
| table _time, TargetUserName, Caller_Computer_Name, host
| sort - _time
```

Check for successful login after failures:

```spl
index=wineventlog (EventCode=4625 OR EventCode=4624)
| stats count by EventCode, Account_Name, Source_Network_Address, host
| sort Account_Name
```

## Escalation Criteria

Escalate immediately if:

- a successful login occurs after repeated failed attempts
- privileged or sensitive accounts are involved
- one source targets multiple users
- activity appears external or unknown
- lockouts affect multiple users or business operations

## Containment / Response

- block or investigate the source IP if clearly malicious
- reset or disable the affected account if compromise is suspected
- notify the security team or system owner
- increase monitoring for follow-on activity
- confirm MFA is enabled where applicable
- review whether password spraying or brute force protections need tuning

## False Positive Checks

- user mistyping password repeatedly
- expired credentials in a service or scheduled task
- mapped drives using outdated passwords
- VPN or SSO issues
- help desk password change activity

## Evidence To Capture

- affected usernames
- source IP addresses
- hosts involved
- event counts
- timestamps
- whether a successful login followed
- related lockout events
- screenshots or saved SPL output

## How I Tested This In My Lab

In my Active Directory lab, I simulated repeated failed login activity and account lockouts, forwarded Windows Security logs into Splunk, and validated detection logic using Event IDs `4625` and `4740`. This let me practice distinguishing user error from suspicious authentication behavior and building a triage workflow that mirrors basic SOC analysis.

## Lessons Learned / Detection Improvements

- correlate `4625` failures with `4624` successes for higher-confidence alerts
- separate service account failures from user account failures
- increase severity when multiple accounts are targeted by one source
- enrich detections with IP reputation or geolocation if available
- suppress known benign authentication noise after validation
