# AWS Security Monitoring System – Troubleshooting Guide

## Project Title

Troubleshooting and Validation of AWS Secret Access Monitoring System

---

# Introduction

During the implementation of the AWS Security Monitoring System, multiple troubleshooting activities were performed to ensure the monitoring workflow functioned correctly. The project involved integrating AWS Secrets Manager, CloudTrail, CloudWatch Logs, Metric Filters, CloudWatch Alarms, and Amazon SNS for real-time alert notifications.

Initially, the monitoring system did not send alert emails after secret access events were triggered. To resolve this issue, systematic troubleshooting was performed across each AWS service involved in the architecture.

---

# Troubleshooting Objectives

The troubleshooting process focused on identifying why email alerts were not received after accessing the secret.

The following components were verified:

* CloudTrail Event Logging
* CloudWatch Log Delivery
* Metric Filter Configuration
* CloudWatch Alarm Status
* SNS Notification Delivery

---

# Troubleshooting Steps Performed

## 1. Verifying CloudTrail Event Recording

### Issue

The monitoring system would fail if CloudTrail did not capture the `GetSecretValue` API activity.

### Verification Steps

1. Opened AWS CloudTrail Console.
2. Navigated to **Event History**.
3. Selected **Event Source** in Lookup Attributes.
4. Entered:

```text
secretsmanager.amazonaws.com
```

5. Checked whether `GetSecretValue` events were present.

### Result

CloudTrail successfully recorded the secret access event whenever the secret was retrieved from Secrets Manager or AWS CLI.

### Learning

CloudTrail is the foundation of the monitoring system because all security events originate from CloudTrail logs.

---

# 2. Verifying CloudWatch Log Delivery

## Issue

Even though CloudTrail captured events, the logs were not initially visible inside CloudWatch Logs.

## Verification Steps

1. Opened the CloudTrail Trail configuration.
2. Verified that CloudWatch Logs integration was enabled.
3. Checked whether:

* Log Group was created
* IAM Role permissions were configured correctly

4. Opened CloudWatch Console.
5. Navigated to:

```text
Logs → Log Groups
```

6. Opened the created log group and verified incoming log streams.

## Result

CloudTrail logs were successfully delivered to CloudWatch Logs after enabling integration correctly.

## Learning

Without CloudWatch Logs integration, Metric Filters and Alarms cannot detect events.

---

# 3. Verifying Metric Filter Configuration

## Issue

The alarm would not trigger if the Metric Filter pattern was incorrect.

## Verification Steps

1. Opened CloudWatch Log Group.
2. Selected:

```text
Actions → Create Metric Filter
```

3. Verified the Filter Pattern:

```text
"GetSecretValue"
```

4. Confirmed:

* Metric Namespace
* Metric Name
* Metric Value
* Default Value

5. Tested the filter pattern against CloudTrail logs.

## Result

The Metric Filter successfully detected `GetSecretValue` events from CloudTrail logs.

## Learning

Metric Filters convert log events into measurable CloudWatch metrics used for monitoring and alerting.

---

# 4. Verifying CloudWatch Alarm Configuration

## Issue

The metric existed, but the alarm remained in the `OK` state.

## Verification Steps

1. Opened CloudWatch Alarms.
2. Verified alarm configuration:

* Metric Namespace
* Metric Name
* Statistic
* Evaluation Period
* Threshold

3. Confirmed threshold settings:

```text
Greater than or equal to 1
```

4. Retrieved the secret again to generate a fresh event.
5. Waited 2–5 minutes for metric evaluation.

## Result

The CloudWatch Alarm successfully changed to the `ALARM` state after detecting the metric.

## Learning

CloudWatch Alarms evaluate metrics periodically, so delays of a few minutes are normal.

---

# 5. Verifying SNS Notification Delivery

## Issue

The alarm triggered successfully, but email notifications were not received.

## Verification Steps

1. Opened Amazon SNS Console.
2. Checked SNS Topic subscription status.
3. Verified that the email subscription was confirmed.
4. Opened email inbox and confirmed the subscription link sent by AWS SNS.
5. Retested the monitoring system.

## Result

SNS successfully delivered email notifications after confirming the subscription.

## Learning

SNS subscriptions remain in a pending state until manually confirmed through email verification.

---

# Final Validation Test

After completing all troubleshooting steps:

1. The secret was accessed again using AWS Secrets Manager and AWS CLI.
2. CloudTrail captured the `GetSecretValue` event.
3. CloudWatch Logs received the logs.
4. Metric Filter detected the event.
5. CloudWatch Alarm entered the `ALARM` state.
6. SNS successfully delivered the email notification.

The monitoring system functioned successfully end-to-end.

---

# Common Issues Identified

| Issue                 | Cause                                | Resolution                 |
| --------------------- | ------------------------------------ | -------------------------- |
| No CloudTrail Events  | Secret not accessed properly         | Retrieve secret again      |
| No Logs in CloudWatch | CloudWatch Logs integration disabled | Enable integration         |
| Alarm not triggering  | Incorrect metric filter pattern      | Use `"GetSecretValue"`     |
| Alarm remains OK      | Evaluation delay                     | Wait 2–5 minutes           |
| No email notification | SNS subscription not confirmed       | Confirm subscription email |

---

# Skills Demonstrated

* AWS Security Monitoring
* CloudTrail Event Analysis
* CloudWatch Log Management
* CloudWatch Alarm Troubleshooting
* SNS Notification Configuration
* AWS IAM Role Validation
* AWS CLI Operations
* Cloud Infrastructure Troubleshooting
* Security Event Monitoring

---

# Conclusion

This troubleshooting process provided hands-on experience in diagnosing and resolving monitoring issues within AWS cloud environments. The project improved practical knowledge of AWS security monitoring, event logging, alert configuration, and cloud troubleshooting workflows. The final solution successfully monitored secret access activities and generated real-time security alerts through Amazon SNS.
