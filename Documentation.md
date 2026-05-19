# PROJECT DOCUMENTATION

# AWS Secret Access Monitoring and Alert System

---

# 1. Project Title

AWS-Based Secret Access Monitoring and Notification System

---

# 2. Introduction

Cloud security is an important aspect of modern IT infrastructure. Organizations frequently store sensitive information such as passwords, API keys, tokens, and database credentials in cloud environments. Monitoring access to these secrets is essential to prevent unauthorized usage and improve security visibility.

This project demonstrates how to build a real-time AWS monitoring and alerting system that tracks secret access activities and automatically sends notifications whenever a secret is accessed.

The implementation uses AWS native services including AWS Secrets Manager, AWS CloudTrail, Amazon CloudWatch, and Amazon SNS.

---

# 3. Problem Statement

Sensitive credentials stored in cloud environments can become security risks if accessed unexpectedly or without authorization. Security administrators require a reliable mechanism to monitor secret access activities and receive immediate alerts whenever confidential information is retrieved.

The objective of this project is to design and implement an automated AWS security monitoring solution that detects secret access events and sends real-time email notifications for auditing and incident response purposes.

---

# 4. Project Objectives

The major objectives of this project are:

* Securely store confidential information using AWS Secrets Manager
* Monitor secret access activities using AWS CloudTrail
* Capture and analyze logs using CloudWatch Logs
* Detect `GetSecretValue` API events using Metric Filters
* Trigger automated alerts using CloudWatch Alarms
* Send email notifications using Amazon SNS
* Implement real-time security monitoring within AWS

---

# 5. Solution Overview

The solution is designed using multiple AWS services working together to provide event-based monitoring and notification capabilities.

The workflow of the system is as follows:

1. Sensitive information is stored securely in AWS Secrets Manager.
2. AWS CloudTrail records all API activities related to secret access.
3. CloudTrail logs are delivered to CloudWatch Logs.
4. A CloudWatch Metric Filter scans the logs for `GetSecretValue` events.
5. A CloudWatch Alarm monitors the generated metric.
6. Whenever the metric threshold is reached, the alarm enters the `ALARM` state.
7. Amazon SNS sends an email notification to the configured recipient.

This creates a complete monitoring pipeline for detecting and responding to secret access events in real time.

---

# 6. AWS Services Used

## 6.1 AWS Secrets Manager

AWS Secrets Manager is used to securely store sensitive information such as passwords, tokens, and confidential application credentials.

### Purpose

* Secure storage of secrets
* Controlled access management
* Encryption of sensitive data

---

## 6.2 AWS CloudTrail

AWS CloudTrail records API activities and user actions performed within the AWS account.

### Purpose

* Monitor API activity
* Track secret access events
* Maintain audit logs

---

## 6.3 Amazon S3

Amazon S3 is used to store CloudTrail log files.

### Purpose

* Long-term storage of audit logs
* Centralized log management

---

## 6.4 Amazon CloudWatch Logs

CloudWatch Logs collects and stores logs from CloudTrail for monitoring and analysis.

### Purpose

* Log collection and management
* Event analysis
* Integration with alarms and metric filters

---

## 6.5 CloudWatch Metric Filter

Metric Filters scan logs and identify matching patterns.

### Purpose

* Detect `GetSecretValue` events
* Convert log events into CloudWatch metrics

---

## 6.6 CloudWatch Alarm

CloudWatch Alarm continuously monitors metrics and triggers alerts when defined thresholds are reached.

### Purpose

* Detect secret access activity
* Generate automated alerts

---

## 6.7 Amazon SNS

Amazon Simple Notification Service (SNS) sends notifications to subscribed users.

### Purpose

* Send real-time email alerts
* Notify administrators about secret access events

---

# 7. System Architecture Workflow

The overall monitoring workflow is shown below:

```text
AWS Secrets Manager
        ↓
AWS CloudTrail
        ↓
CloudWatch Logs
        ↓
Metric Filter
        ↓
CloudWatch Alarm
        ↓
Amazon SNS Email Notification
```

---

# 8. Project Implementation Steps

## Step 1: Create a Secret

* Created a secret in AWS Secrets Manager
* Stored a sample confidential value
* Assigned the secret name:

```text
TopSecretInfo
```

---

## Step 2: Configure CloudTrail

* Created a CloudTrail trail named:

```text
secrets-manager-trail
```

* Enabled management event logging
* Configured S3 bucket storage for logs

---

## Step 3: Access the Secret

The secret was accessed using:

* AWS Management Console
* AWS CLI through CloudShell

Example AWS CLI command:

```bash
aws secretsmanager get-secret-value --secret-id "TopSecretInfo" --region us-east-1
```

---

## Step 4: Verify CloudTrail Events

* Opened CloudTrail Event History
* Filtered events using:

```text
secretsmanager.amazonaws.com
```

* Verified the presence of:

```text
GetSecretValue
```

events.

---

## Step 5: Configure CloudWatch Logs

* Enabled CloudWatch Logs integration in CloudTrail
* Created a Log Group
* Configured IAM Role permissions

---

## Step 6: Create Metric Filter

A Metric Filter was configured to detect secret access activity.

### Filter Configuration

| Field            | Value                  |
| ---------------- | ---------------------- |
| Filter Pattern   | `"GetSecretValue"`     |
| Filter Name      | `GetSecretValue`       |
| Metric Namespace | `SecurityMetrics`      |
| Metric Name      | `SecretAccessDetected` |
| Metric Value     | `1`                    |
| Default Value    | `0`                    |

---

## Step 7: Configure CloudWatch Alarm

The alarm was created using the generated metric.

### Alarm Configuration

| Field               | Value                      |
| ------------------- | -------------------------- |
| Metric Namespace    | `SecurityMetrics`          |
| Metric Name         | `SecretAccessDetected`     |
| Statistic           | Average                    |
| Evaluation Period   | 5 Minutes                  |
| Threshold Type      | Static                     |
| Alarm Condition     | Greater than or equal to 1 |
| Notification Action | SNS Topic: SecurityAlarms  |

---

## Step 8: Configure SNS Notifications

* Created SNS Topic:

```text
SecurityAlarms
```

* Added email subscription
* Confirmed SNS subscription through email verification

---

# 9. Testing and Validation

The monitoring system was tested by retrieving the secret value again from AWS Secrets Manager.

The following actions occurred successfully:

1. CloudTrail captured the `GetSecretValue` event.
2. CloudWatch Logs received the event logs.
3. Metric Filter detected the matching event pattern.
4. CloudWatch Alarm entered the `ALARM` state.
5. Amazon SNS delivered the email notification successfully.

This confirmed that the monitoring workflow functioned correctly from end to end.

---

# 10. Troubleshooting Performed

Several troubleshooting activities were performed during implementation.

## Issues Identified

* CloudTrail logs not visible in CloudWatch initially
* Alarm remained in `OK` state
* SNS notifications were delayed
* Metric Filter pattern validation issues

## Resolutions

* Verified CloudTrail integration with CloudWatch Logs
* Reconfigured Metric Filter pattern
* Confirmed SNS email subscription
* Retrieved the secret multiple times to generate fresh events
* Waited for CloudWatch evaluation period

---

# 11. Final Result

The AWS Secret Access Monitoring System was successfully implemented and tested.

The project achieved the following outcomes:

* Real-time monitoring of secret access events
* Automatic detection of sensitive API activities
* Email notification alerts through SNS
* Centralized logging and event analysis
* Improved security visibility within AWS

---

# 12. Skills Demonstrated

This project helped develop practical knowledge in the following areas:

* AWS Security Monitoring
* AWS CloudTrail Logging
* CloudWatch Monitoring and Alerting
* Amazon SNS Notification Setup
* AWS CLI Operations
* Event-Based Monitoring
* Security Auditing
* Identity and Access Monitoring
* Cloud Troubleshooting
* Log Analysis and Filtering

---

# 13. Resource Cleanup

To avoid unnecessary AWS charges, the following resources should be deleted after project completion:

* AWS Secrets Manager Secret
* CloudTrail Trail
* Amazon S3 Bucket
* CloudWatch Alarm
* CloudWatch Log Group
* Metric Filter
* SNS Topic and Subscription

---

# 14. Conclusion

This project successfully demonstrated how AWS services can be integrated to build a real-time security monitoring and alerting solution. By combining Secrets Manager, CloudTrail, CloudWatch, and SNS, the system effectively monitored sensitive secret access activities and delivered automated notifications whenever a security-related event occurred.

The project provided valuable hands-on experience in cloud security, monitoring, logging, troubleshooting, and event-driven alert management within AWS environments.
