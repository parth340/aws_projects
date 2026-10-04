# Monitoring and Logging Platform on AWS

## Overview

This project demonstrates the implementation of a centralized Monitoring and Logging Platform on AWS using Amazon EC2, CloudWatch, SNS, CloudTrail, and IAM.

The platform provides real-time visibility into system performance, application health, operational logs, and AWS account activities. It enables proactive monitoring, automated alerting, faster troubleshooting, and improved operational reliability.

This solution closely resembles the monitoring architecture commonly used by Cloud Engineers, DevOps Engineers, and Site Reliability Engineers (SREs) in production environments.

---

## Project Objectives

The platform was designed to:

- Monitor infrastructure performance.
- Collect and centralize application logs.
- Visualize operational metrics.
- Generate automated alerts.
- Improve troubleshooting capabilities.
- Track AWS account activities and changes.
- Build foundational observability skills on AWS.

---

## Architecture

### Components

#### Amazon EC2

Hosts the Apache web application that serves as the monitoring target.

#### CloudWatch Agent

Collects:

- CPU Metrics
- Memory Metrics
- Disk Utilization Metrics
- Swap Usage Metrics
- System Logs
- Apache Logs

#### Amazon CloudWatch

Provides:

- Metrics Collection
- Log Aggregation
- Dashboards
- Alarms

#### Amazon SNS

Sends notifications whenever an alarm is triggered.

#### AWS CloudTrail

Provides auditing and visibility into AWS API activities.

---

## AWS Services Used

- Amazon EC2
- Amazon CloudWatch
- CloudWatch Agent
- CloudWatch Dashboards
- CloudWatch Alarms
- Amazon SNS
- IAM
- AWS CloudTrail
- Amazon Linux 2023
- Apache HTTP Server

---

## Monitoring Capabilities

### Infrastructure Monitoring

Monitored Metrics:

- CPU Utilization
- Memory Utilization
- Disk Utilization
- Swap Usage
- Network Traffic

### Log Monitoring

Collected Logs:

- /var/log/messages
- /var/log/httpd/access_log
- /var/log/httpd/error_log

---

## Dashboard

A centralized CloudWatch Dashboard was created to visualize:

- CPU Utilization
- Memory Usage
- Disk Usage
- Network Traffic
- System Health

---

## Alerting

CloudWatch Alarm configured:

### CPU Alarm

Condition:

```text
CPU Utilization > 80%
```

Action:

```text
Publish notification to SNS
```

Notification Method:

```text
Email
```

---

## Audit Logging

CloudTrail Event History was used to monitor:

- AWS API Calls
- Resource Changes
- User Activities
- Account Operations

This provides auditability and security visibility.

---

## Testing Performed

### Web Server Validation

Verified successful HTTP responses from Apache server.

### CloudWatch Metrics Validation

Confirmed metric collection for:

- CPU
- Memory
- Disk
- Swap

### Log Collection Validation

Confirmed ingestion of:

- System logs
- Apache access logs
- Apache error logs

### Alert Validation

Generated CPU load using:

```bash
stress --cpu 2 --timeout 300
```

Verified:

- Alarm state transition
- SNS notification delivery

---

## Security Best Practices

- IAM Role-based authentication
- No hardcoded AWS credentials
- Principle of least privilege
- CloudTrail auditing enabled
- Centralized logging

---

## Business Benefits

This solution helps organizations:

- Detect issues proactively
- Reduce Mean Time To Resolution (MTTR)
- Improve system reliability
- Centralize operational visibility
- Monitor application health
- Improve security auditing

---

## Future Enhancements

Planned improvements include:

- CloudWatch Log Insights
- Custom Application Metrics
- AWS Lambda Auto-remediation
- Amazon OpenSearch Integration
- Grafana Dashboards
- Multi-Instance Monitoring
- Slack / Microsoft Teams Integration
- Custom Security Dashboards

---

## Project Structure

```text
monitoring-logging-platform/
│
├── README.md
├── DEPLOYMENT-GUIDE.md
├── architecture-diagram.png
│
├── screenshots/
│   ├── ec2-instance.png
│   ├── cloudwatch-agent.png
│   ├── cloudwatch-metrics.png
│   ├── cloudwatch-logs.png
│   ├── dashboard.png
│   ├── alarm.png
│   ├── sns-topic.png
│   ├── sns-email-alert.png
│   └── cloudtrail.png
│
└── scripts/
```

---

## Skills Demonstrated

- AWS Cloud Monitoring
- CloudWatch Metrics
- CloudWatch Logs
- CloudWatch Dashboards
- CloudWatch Alarms
- SNS Notifications
- CloudTrail Auditing
- Linux Administration
- Apache Administration
- IAM Roles
- Observability
- Monitoring & Alerting

---

## Author

**Parth Sawant**
