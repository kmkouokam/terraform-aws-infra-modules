# S3 Bucket for CloudTrail Logs Module

This Terraform module provisions a secure and lifecycle-managed Amazon S3 bucket used for storing **AWS CloudTrail logs**. It includes versioning, lifecycle policies, and an S3 bucket policy that grants CloudTrail write access while maintaining bucket ownership control.

---

## 📁 Resources Provisioned

### 🪣 S3 Bucket
- **Resource:** `aws_s3_bucket.logs`
- **Description:** The main bucket to store CloudTrail logs.
- **Options:**
  - `force_destroy = true`: Allows Terraform to delete non-empty buckets
  - Tags are passed via input variable

---

### 🧬 S3 Versioning
- **Resource:** `aws_s3_bucket_versioning.logs`
- **Status:** Enabled
- **Purpose:** Maintains previous versions of modified/deleted objects for audit/compliance.

---

### 📆 Lifecycle Configuration
- **Resource:** `aws_s3_bucket_lifecycle_configuration.logs`
- **Rules:**
  - **Non-current version expiration:** Deletes old object versions after **30 days**
  - **Current object expiration:** Deletes current objects older than **365 days**
  - **Abort incomplete multipart uploads:** After **7 days**

---

### 🔐 S3 Bucket Policy for CloudTrail
- **Resource:** `aws_s3_bucket_policy.cloudtrail_logs`
- **Grants CloudTrail the following permissions:**
  - `s3:GetBucketAcl` — to verify write permissions
  - `s3:PutObject` — to store log files
- **Condition:** Ensures logs are written with ACL: `bucket-owner-full-control`

---

## 🔧 Required Variables

| Name         | Description                                  | Type     |
|--------------|----------------------------------------------|----------|
| `bucket_name`| Name of the S3 bucket                        | `string` |
| `tags`       | Tags to apply to the S3 bucket               | `map`    |

---

## 🚀 Example Usage

```hcl
module "s3_logs_bucket" {
  source      = "./modules/s3_logs"   #update this as needed
  bucket_name = "my-org-cloudtrail-logs"
  tags = {
    Environment = "prod"
    Team        = "security"
  }
}
```

---

## ✅ Features

- ✅ CloudTrail-compliant bucket policy
- ✅ Secure write permissions with ACL enforcement
- ✅ Versioning enabled
- ✅ Lifecycle rules for cost optimization
- ✅ Automatic cleanup of incomplete uploads

---

## 📄 License

This project is licensed under the **Mozilla Public License 2.0** (MPL-2.0).  
See the [LICENSE](./LICENSE) file for full details.
