# AWS Security Monitoring System

This project demonstrates how to build a security monitoring and alerting system in AWS using:

- AWS Secrets Manager
- AWS CloudTrail
- Amazon CloudWatch
- Amazon SNS
- Amazon S3
- AWS CLI

## Project Objective

The goal of this project is to monitor access to sensitive secrets stored in AWS Secrets Manager and receive alerts whenever a secret is accessed.

## Architecture

1. Store secret in AWS Secrets Manager
2. Track events using AWS CloudTrail
3. Send logs to CloudWatch Logs
4. Create Metric Filter for GetSecretValue
5. Trigger CloudWatch Alarm
6. Send Email Notifications using SNS

## Services Used

- AWS Secrets Manager
- AWS CloudTrail
- Amazon CloudWatch
- Amazon SNS
- Amazon S3
- IAM
- AWS CloudShell

## AWS CLI Command Used

aws secretsmanager get-secret-value --secret-id "TopSecretInfo" --region ap-south-1

## Outcome

Successfully built a real-time AWS security monitoring system that detects and alerts whenever secrets are accessed.

## Author

ANUSUYA K
