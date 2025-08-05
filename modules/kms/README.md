# AWS KMS Keys Module

This Terraform module provisions AWS Key Management Service (KMS) keys for encrypting data used by key AWS services such as **Secrets Manager**, **RDS MySQL**, and **Amazon S3** (e.g., CloudTrail logs). It includes key rotation and fine-grained policies to securely delegate encryption responsibilities to the respective services.

---

## 🔐 KMS Keys Provisioned

### 1. 🔑 Secrets Manager KMS Key
- **Resource:** `aws_kms_key.secrets_manager`
- **Description:** Key for encrypting secrets stored in AWS Secrets Manager
- **Key Policy Includes:**
  - Full access for the AWS account root
  - Permissions for `secretsmanager.amazonaws.com` to use the key (Encrypt, Decrypt, GenerateDataKey, DescribeKey)

---

### 2. 🛢️ RDS MySQL KMS Key
- **Resource:** `aws_kms_key.rds`
- **Description:** Key for encrypting RDS MySQL data
- **Key Policy Includes:**
  - Full access for the AWS account root
  - Permissions for `rds.amazonaws.com` to use the key

---

### 3. 📦 S3 (CloudTrail Logs) KMS Key
- **Resource:** `aws_kms_key.s3`
- **Description:** Key for encrypting S3 buckets, such as those storing CloudTrail logs
- **Key Policy Includes:**
  - Full access for the AWS account root
  - Permissions for `s3.amazonaws.com` to use the key

---

## 🔄 Features

- ✅ Key rotation enabled for all keys
- 🧹 Deletion window of 20 days (allows recovery if keys are accidentally scheduled for deletion)
- 📜 Custom key policies to allow only trusted AWS services to use the keys

---

## 📌 Usage

```hcl
module "kms_keys" {
  source = "./modules/kms"  # Update path as needed
}
```

---

## ✅ Requirements

- Terraform v1.0+
- AWS Provider v4.0+

---

## 📄 License

This project is licensed under the **Mozilla Public License 2.0** (MPL-2.0).  
See the [LICENSE](./LICENSE) file for full details.
