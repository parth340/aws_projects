# Serverless Cost Optimizer & Security Auditor on AWS

## Overview

The Serverless Cost Optimizer & Security Auditor is an automated AWS governance solution that continuously scans cloud resources for potential cost-saving opportunities and security risks.

The solution uses AWS Lambda to perform automated audits, Amazon EventBridge to schedule recurring scans, Amazon SNS to deliver notifications, and Amazon CloudWatch for monitoring and observability.

By automating cost and security assessments, organizations can proactively identify unused resources, reduce unnecessary spending, and mitigate security risks without manual intervention.

---

## Business Problem

Cloud environments often accumulate unused resources that increase operational costs and expose security vulnerabilities.

Common examples include:

- Unattached EBS volumes generating unnecessary charges
- Unused Elastic IPs consuming resources
- Security Groups with unrestricted internet access
- Lack of regular infrastructure audits
- Delayed visibility into cost optimization opportunities

This project solves these challenges through automated serverless auditing and reporting.

---

## Solution Architecture

The solution consists of:

- AWS Lambda for automated audits
- IAM Role for controlled AWS access
- Amazon SNS for email notifications
- Amazon EventBridge for scheduled execution
- Amazon CloudWatch for logging and monitoring
- Amazon EC2 resource inspection

---

## Key Features

### Cost Optimization Checks

- Detect unattached EBS volumes
- Identify unused Elastic IP addresses
- Highlight unnecessary AWS resource consumption

### Security Auditing

- Detect Security Groups exposing ports to 0.0.0.0/0
- Generate security findings automatically
- Improve infrastructure visibility

### Event-Driven Automation

- Fully serverless architecture
- No infrastructure management required
- Scheduled automated execution

### Reporting

- Consolidated audit reports
- Email notifications via SNS
- Real-time findings delivery

### Monitoring

- Centralized Lambda logs
- Execution monitoring through CloudWatch
- Troubleshooting and audit visibility

---

## AWS Services Used

| Service | Purpose |
|----------|----------|
| AWS Lambda | Audit execution engine |
| IAM | Security permissions |
| Amazon SNS | Email notifications |
| Amazon EventBridge | Scheduled execution |
| Amazon CloudWatch | Monitoring and logs |
| Amazon EC2 | Resource inspection |

---

## Audit Checks

### Cost Audits

✅ Unattached EBS Volumes

✅ Unused Elastic IPs

### Security Audits

✅ Security Groups open to the Internet (0.0.0.0/0)

---

## Automated Workflow

1. EventBridge triggers Lambda on schedule.
2. Lambda scans AWS resources.
3. Findings are compiled into a report.
4. Report is sent to SNS.
5. SNS emails the report to subscribers.
6. CloudWatch stores execution logs.

---

## Sample Report

AWS Cost & Security Audit Report

=== COST FINDINGS ===

Unattached EBS Volume: vol-xxxxxxxx

Unused Elastic IP: xx.xx.xx.xx

=== SECURITY FINDINGS ===

Open Security Group: webserver-sg

---

## Testing

The solution was validated by:

- Creating unattached EBS volumes
- Allocating unused Elastic IPs
- Creating publicly accessible Security Groups
- Executing Lambda manually
- Verifying email delivery
- Reviewing CloudWatch logs

---

## Benefits

- Automated governance
- Reduced cloud spend
- Improved security posture
- Faster risk identification
- Minimal operational overhead
- Fully serverless implementation

---

## Future Enhancements

- Resource tagging compliance checks
- AWS Trusted Advisor integration
- S3 bucket public access audits
- IAM security posture assessments
- Cost Explorer integration
- Slack and Microsoft Teams notifications
- Multi-account auditing using Organizations
- Automated remediation using Systems Manager

---

## Author
Parth Sawant
