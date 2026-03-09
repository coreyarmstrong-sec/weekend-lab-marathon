# Suspicious Group Membership Change Triage Runbook

## Purpose

This runbook is used to investigate alerts involving users being added to sensitive or privileged Active Directory groups. The goal is to quickly determine whether the change is authorized administrative activity or potential privilege escalation.

## Environment Context

This runbook is based on my lab environment:

- Windows Server 2022 Active Directory domain
- 40+ test users created with PowerShell
- Windows Security logs forwarded into Splunk
- Detection focus on Event ID `4728`
- Monitoring for privileged group membership changes and suspicious account activity

## Alert Logic

Trigger an alert when a user is added to a security-sensitive group, especially groups associated with elevated privileges or administrative access.

Example logic in Splunk:

```spl
index=wineventlog EventCode=4728
| table _time, SubjectUserName, MemberName, Group_Name, host
| sort - _time
```

## Likely Causes

- legitimate administrator action
- onboarding or role change
- help desk provisioning
- temporary elevated access request
- malicious privilege escalation
- compromised admin or delegated account misuse

## Required Data Sources

- Splunk Windows Security logs
- Event ID `4728`
- actor account that made the change
- user added to the group
- group name
- source host
- timestamp
- related logon activity for the actor account
- approval ticket or change record if available

## Investigation Steps

1. Identify the user added to the group, the actor account that performed the change, the target group, and the time of the event.
2. Determine whether the affected group is privileged or operationally sensitive.
3. Check whether the actor account normally performs administrative changes.
4. Review whether the action matches an approved ticket, onboarding request, or role change.
5. Review recent logon activity for the actor account to confirm whether the login pattern looks normal.
6. Check the source host used for the change and confirm whether it is an expected admin workstation or server.
7. Review follow-on actions by the newly added account to determine whether privileged activity occurred shortly after the change.
8. Decide whether the change is authorized, suspicious, or clearly malicious.

## Splunk Queries

Review group membership changes:

```spl
index=wineventlog EventCode=4728
| table _time, SubjectUserName, MemberName, Group_Name, host
| sort - _time
```

Review recent actor account activity:

```spl
index=wineventlog SubjectUserName="<actor_account>"
| table _time, EventCode, SubjectUserName, host
| sort - _time
```

Review activity from the added account after the change:

```spl
index=wineventlog Account_Name="<added_user>" OR TargetUserName="<added_user>"
| table _time, EventCode, host, Account_Name, TargetUserName
| sort - _time
```

## Escalation Criteria

Escalate immediately if:

- no approved change record exists
- the affected group is privileged
- the actor account is unusual for administrative work
- the source host is unexpected
- the added account performs privileged activity shortly after the change

## Containment / Response

- remove unauthorized group membership
- disable or reset credentials for affected accounts if compromise is suspected
- isolate the source host if malicious activity is confirmed
- notify AD administrators and security leadership
- monitor for any additional privilege changes or suspicious logons

## False Positive Checks

- planned onboarding or offboarding
- approved role change
- help desk administrative support action
- temporary access request
- lab or test environment changes

## Evidence To Capture

- group name
- user added
- actor account
- source host
- timestamp
- related ticket or approval reference
- any follow-on privileged actions
- screenshots or saved SPL output

## How I Tested This In My Lab

In my Active Directory lab, I used Windows Security logs forwarded to Splunk to monitor group membership changes using Event ID `4728`. I focused on identifying the actor account, the user added, and the target group, then reviewing surrounding events to determine whether the action appeared normal or suspicious. This helped me practice how privilege-related changes should be validated in a SOC workflow.

## Lessons Learned / Detection Improvements

- prioritize alerts on privileged groups first
- enrich group change detections with ticket or approval context where possible
- correlate `4728` events with unusual logon behavior by the actor account
- raise severity for after-hours changes or changes from unexpected hosts
- add detection logic for related group membership event IDs as the environment grows
