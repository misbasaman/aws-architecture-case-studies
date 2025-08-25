# HIPAA Notes – AWS Telehealth Architecture

## Overview
This document outlines how the proposed telehealth architecture addresses **HIPAA compliance** within the AWS Shared Responsibility Model. It highlights what AWS is responsible for, and what the customer (you/your org) must enforce.

---

## HIPAA Shared Responsibility Model

- **AWS Responsibility**  
  - Physical security of data centers  
  - Underlying infrastructure security (compute, storage, networking)  
  - HIPAA-eligible service compliance when covered by a **signed Business Associate Addendum (BAA)**  

- **Customer Responsibility**  
  - Correctly configuring HIPAA-eligible services  
  - Ensuring PHI is not placed in non-eligible services  
  - Implementing access controls, audit logging, encryption, and monitoring  
  - User authentication, RBAC, and break-glass procedures  
  - Incident response and breach notification  

---

## HIPAA-Eligible Services in this Architecture
- **Amazon CloudFront** (with AWS WAF)  
- **Amazon Cognito**  
- **Amazon API Gateway**  
- **AWS Lambda**  
- **Amazon Aurora (RDS, PostgreSQL)**  
- **Amazon HealthLake** (FHIR-compliant)  
- **Amazon S3 (with KMS encryption)**  
- **Amazon Chime SDK** (HIPAA-eligible under BAA)  
- **Amazon SNS/SQS**  
- **AWS IAM, KMS, Secrets Manager**  
- **AWS CloudTrail, CloudWatch, Config, GuardDuty, Security Hub, Macie**  

---

## Data Protection Controls

- **Encryption in Transit:** TLS 1.2+ enforced end-to-end  
- **Encryption at Rest:** AWS KMS CMKs used across S3, Aurora, HealthLake, Chime artifacts (if stored)  
- **Key Management:** Customer-managed CMKs with strict IAM & rotation policies  
- **Network Isolation:**  
  - VPC private subnets for databases and Lambda ENIs  
  - VPC Endpoints for S3, HealthLake, and API Gateway → private data paths  

---

## Logging & Auditing

- **CloudTrail**: Logs all API calls, delivered to encrypted S3  
- **CloudWatch Logs**: Application logs for Lambda & API Gateway  
- **AWS Config**: Tracks resource drift & misconfiguration  
- **Security Hub**: Aggregates findings across GuardDuty, Config, and Macie  
- **HealthLake**: Provides FHIR resource versioning & change history for clinical audit  

---

## Key Caveats

- **Must sign a BAA in AWS Artifact** before using PHI in workloads  
- **HIPAA-eligible ≠ HIPAA-compliant** — correct configurations are mandatory  
- **De-identification** is recommended for analytics workflows (QuickSight, Athena)  
- **Data retention & deletion policies** must be explicitly defined in the application layer  

---

## References
- [AWS HIPAA Eligibility and Services](https://aws.amazon.com/compliance/hipaa-eligible-services-reference/)  
- [AWS Artifact (BAA)](https://aws.amazon.com/artifact/)  
- [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)  

