# Lab 3: AWS Cloud Security Fundamentals

## Objective
Configure real AWS security controls demonstrating cloud security posture management (CSPM) skills for Security Engineer roles.

## Environment
- **Platform:** Amazon Web Services (Free Tier)
- **Services:** IAM, CloudTrail, CloudWatch, AWS Config, EC2

## What I Built

### IAM — Identity & Access Management
- Created admin user with SecurityAudit policy
- Created restricted user with EC2ReadOnly policy
- Enabled MFA on both accounts
- Configured password policy: 14 char minimum, complexity required, 90-day rotation

### CloudTrail — Audit Logging
- Created multi-region trail: LabMarathonTrail
- Logs stored in S3 bucket with log file validation enabled
- Integrated with CloudWatch Logs for real-time monitoring
- Log file validation detects tampering

### CloudWatch — Root Account Alert
- Created alarm triggered on any root account usage
- SNS topic configured with email notification
- Threshold: count >= 1 (any root login = immediate alert)

### AWS Config — Compliance Monitoring
Configured 3 managed compliance rules:
| Rule | What It Checks |
|------|----------------|
| restricted-ssh | No 0.0.0.0/0 on port 22 |
| iam-password-policy | Password policy meets requirements |
| cloudtrail-enabled | CloudTrail active in all regions |

## Key Security Concepts Demonstrated
- Principle of least privilege via IAM policies
- Defense in depth — MFA + password policy + audit logging
- Cloud Security Posture Management (CSPM)
- Automated compliance monitoring
- Incident detection via CloudWatch alerting

## Screenshots
![IAM Users](./screenshots/iam-users.png)
![CloudTrail Trail](./screenshots/cloudtrail.png)

## Resume Bullet
Configured AWS cloud security controls including IAM least-privilege policies with MFA enforcement, multi-region CloudTrail audit logging with S3 storage and tamper detection, CloudWatch root account alerting via SNS, and AWS Config compliance monitoring across 3 managed rules demonstrating CSPM experience.
