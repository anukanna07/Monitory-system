# Beginner-Friendly Implementation Guide

## Step 1: Store a Secret Using AWS Secrets Manager

1. Sign in to the AWS Management Console.
2. Search for and open **AWS Secrets Manager**.
3. Click **Store a new secret**.
4. Select **Other type of secret**.
5. Enter the following key-value pair:

   * Key: `The Secret is`
   * Value: `Your sample secret value`
6. Click **Next**.
7. Enter the secret name:

```text
TopSecretInfo
```

8. Skip the rotation configuration.
9. Review the settings and click **Store**.

---

# Step 2: Configure AWS CloudTrail

1. Open the **CloudTrail** service.
2. Navigate to **Trails**.
3. Click **Create trail**.
4. Enter the trail name:

```text
secrets-manager-trail
```

5. Create a new Amazon S3 bucket for storing logs.
6. Disable **Log file SSE-KMS encryption** if you want to avoid additional KMS charges.
7. Keep **Management Events** enabled.
8. Ensure both **Read** and **Write** events are selected.
9. Click **Create trail**.

---

# Step 3: Access the Secret

## Access Through AWS Console

1. Open **Secrets Manager**.
2. Select the secret:

```text
TopSecretInfo
```

3. Click **Retrieve secret value**.

---

## Access Through AWS CloudShell

Run the following AWS CLI command:

```bash
aws secretsmanager get-secret-value --secret-id "TopSecretInfo" --region your-region-code
```

Example:

```bash
aws secretsmanager get-secret-value --secret-id "TopSecretInfo" --region us-east-1
```

---

# Step 4: Validate CloudTrail Events

1. Open **CloudTrail**.
2. Select **Event history** from the left navigation pane.
3. Choose **Event source** under Lookup attributes.
4. Search for:

```text
secretsmanager.amazonaws.com
```

5. Verify the following event appears:

```text
GetSecretValue
```

This confirms that CloudTrail successfully captured the secret access activity.

---

# Step 5: Enable CloudWatch Logs Integration

1. Navigate to:

```text
CloudTrail → Trails
```

2. Open:

```text
secrets-manager-trail
```

3. Scroll down to the **CloudWatch Logs** section.
4. Click **Edit**.
5. Enable **CloudWatch Logs**.
6. Create a new Log Group.
7. Create a new IAM Role for CloudTrail.
8. Save the configuration changes.

---

# Step 6: Configure CloudWatch Metric Filter

1. Open **Amazon CloudWatch**.
2. Navigate to:

```text
Logs → Log Groups
```

3. Open the CloudTrail log group.
4. Select:

```text
Actions → Create Metric Filter
```

5. Use the following filter pattern:

```text
"GetSecretValue"
```

6. Configure the metric details:

| Field            | Value              |
| ---------------- | ------------------ |
| Filter Name      | GetSecretValue     |
| Metric Namespace | SecurityMetrics    |
| Metric Name      | Secret is accessed |
| Metric Value     | 1                  |
| Default Value    | 0                  |

7. Create the metric filter.

---

# Step 7: Create a CloudWatch Alarm

1. Select the created metric filter.
2. Click **Create alarm**.
3. Configure the alarm with the following settings:

| Configuration | Value                      |
| ------------- | -------------------------- |
| Statistic     | Average                    |
| Period        | 5 Minutes                  |
| Threshold     | Greater than or equal to 1 |

4. Create or choose an SNS topic:

```text
SecurityAlarms
```

5. Enter your email address for notifications.
6. Provide the alarm name:

```text
Secret is accessed
```

7. Click **Create alarm**.

---

# Step 8: Confirm the SNS Subscription

1. Open your email inbox.
2. Locate the email sent by **AWS Notifications**.
3. Click the **Confirm subscription** link.

This activates email notifications for alarm alerts.

---

# Step 9: Test the Monitoring System

1. Open **Secrets Manager** again.
2. Select:

```text
TopSecretInfo
```

3. Click **Retrieve secret value** once more.
4. Wait approximately 2–5 minutes.
5. Verify:

   * CloudWatch Alarm enters the `ALARM` state
   * SNS email notification is received successfully

---

# Step 10: Resource Cleanup

To avoid unnecessary AWS charges, delete the following resources after completing the project:

* AWS Secrets Manager Secret
* CloudTrail Trail
* Amazon S3 Bucket
* CloudWatch Log Group
* CloudWatch Alarm
* SNS Topic and Subscription

---

# Final Outcome

This project demonstrates how to build a real-time AWS security monitoring solution that detects secret access activity and sends automated email alerts using CloudTrail, CloudWatch, and Amazon SNS.
