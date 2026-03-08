# Lab 1: Splunk SIEM Setup & Detection Engineering

## Objective
Deploy Splunk Enterprise on Ubuntu Server, ingest Linux authentication logs, and build real SOC detection alerts.

## Environment
- **VM:** Ubuntu Server 22.04 on Proxmox
- **Tool:** Splunk Enterprise 10.2.1
- **Log Source:** /var/log/auth.log (linux_secure sourcetype)

## What I Built
- Deployed Splunk from scratch on a headless Ubuntu VM
- Configured file monitoring input for Linux auth logs
- Authored 3 SPL detection queries

## Detection Queries

**Brute Force SSH Detection:**
```spl
index=main sourcetype=linux_secure "Failed password" 
| stats count by host 
| where count > 5
```

**Unauthorized Root Session:**
```spl
index=main sourcetype=linux_secure "session opened for user root"
| table _time, host, _raw
```

**New User Account Created:**
```spl
index=main sourcetype=linux_secure "new user"
| table _time, host, _raw
```

## Screenshots
![Splunk Dashboard](./screenshots/splunk-dashboard.png)
![Auth Log Events](./screenshots/auth-log-events.png)

## Resume Bullet
Deployed Splunk Enterprise SIEM on Ubuntu Server; ingested Linux auth logs; authored SPL detection rules for SSH brute force, unauthorized root access, and account creation events.
