# KMS Keys Terraform Module

This Terraform module provisions AWS KMS (Key Management Service) keys and associated key policies for encrypting critical resources such as Secrets Manager secrets, RDS MySQL databases, and S3 buckets (e.g., CloudTrail logs).

---

## Features

- Creates a KMS key for Secrets Manager encryption with key rotation enabled
- Creates a KMS key for RDS MySQL encryption with key rotation enabled
- Creates a KMS key for S3 encryption (e.g., CloudTrail logs) with key rotation enabled
- Defines appropriate key policies granting access to the root AWS account and respective AWS services (Secrets Manager, RDS, S3)
- Sets deletion window to 20 days for safe key removal

---

## Resources Created

- `aws_kms_key.secrets_manager`
- `aws_kms_key_policy.secrets_manager_policy`
- `aws_kms_key.rds`
- `aws_kms_key_policy.rds_policy`
- `aws_kms_key.s3`
- `aws_kms_key_policy.s3_policy`

---

## Inputs

- **env** (string): The environment for tagging and naming (e.g., dev, prod).

---

## Outputs

- **secrets_manager_kms_key_id**: KMS key ID for Secrets Manager
- **rds_kms_key_id**: KMS key ID for RDS MySQL
- **s3_kms_key_id**: KMS key ID for S3 encryption
- **rds_kms_key_arn**: ARN of the KMS key for RDS MySQL

---

## Usage Example

```hcl
module "kms" {
  source = "github.com/kmkouokam/infra-modules//aws/modules/kms"
  env    = var.env
}
 