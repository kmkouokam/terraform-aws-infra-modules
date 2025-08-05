# AWS CloudTrail Module

This Terraform module provisions an **AWS CloudTrail** trail that captures all management and data events across multiple regions. Logs are stored in an S3 bucket with log file validation enabled for integrity.

---

## 📦 Resource Provisioned

### 🔍 `aws_cloudtrail.main`
- **Trail Name:** `${var.env}-cloudtrail`
- **Multi-region:** Enabled (captures events from all AWS regions)
- **Log File Validation:** Enabled (cryptographic digest of log files)
- **Global Service Events:** Included (e.g., IAM, STS)
- **Logging:** Enabled
- **Destination:** S3 bucket provided by `var.s3_bucket_name`

#### 📄 Event Selector
- **Read/Write Type:** `All`
- **Management Events:** Included
- **Data Events for S3:**
  - Type: `AWS::S3::Object`
  - Values: Logs object-level API activity in the given S3 bucket

---

## 🔧 Input Variables

| Name              | Description                                     | Type     | Required |
|-------------------|-------------------------------------------------|----------|----------|
| `env`             | Environment prefix for naming (e.g., `dev`, `prod`) | `string` | ✅ Yes    |
| `s3_bucket_name`  | Name of the S3 bucket where CloudTrail stores logs | `string` | ✅ Yes    |
| `tags`            | Key-value tags applied to the CloudTrail trail    | `map`    | ✅ Yes    |

---

## 🚀 Example Usage

```hcl
module "cloudtrail" {
  source          = "github.com/kmkouokam/infra-modules//aws/modules/cloudtrail"
  env             = "your env"
  s3_bucket_name  = "your-s3-bucket-name"
  tags = {
    Environment = "your env"
    Team        = "your team"
  }
}
```

---

## ✅ Features

- ✅ Captures both **management** and **S3 data** events
- ✅ Logs events across **all AWS regions**
- ✅ Supports **log file integrity validation**
- ✅ Uses customer-defined **S3 bucket for log storage**
- ✅ Fully **taggable** for cost and resource tracking

---

## 📄 License

This project is licensed under the **Mozilla Public License 2.0** (MPL-2.0).  
See the [LICENSE](./LICENSE) file for full details.
