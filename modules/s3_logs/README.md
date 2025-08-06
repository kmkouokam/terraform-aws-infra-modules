 # S3 Logs Terraform Module

This Terraform module provisions an Amazon S3 bucket for log storage, following best practices for security, versioning, and lifecycle management. It is designed for storing logs from services like AWS CloudTrail.

---

## Features

- Creates an S3 bucket with customizable name and tags
- Enables versioning to preserve object history
- Configures lifecycle rules to:
  - Delete non-current object versions after 30 days
  - Delete current objects after 1 year
  - Abort incomplete multipart uploads after 7 days
- Adds an S3 bucket policy to allow secure log delivery from AWS CloudTrail
- Outputs useful identifiers such as bucket name and ARN

---

## Usage Example

```hcl
module "s3_logs_bucket" {
  source = "github.com/kmkouokam/infra-modules//aws/modules/s3_logs"

  env         = var.env
  bucket_name = "my-cloudtrail-logs-name"
  tags = {
    Environment = var.env
    Project     = "infra"
  }
}
```

---

## Input Variables

- `env`: The environment for which the bucket is being provisioned (e.g., `dev`, `prod`).
- `bucket_name`: The name of the S3 bucket to be created.
- `tags`: A map of tags to apply to the bucket and related resources.

---

## Outputs

- `bucket_name`: The name of the created S3 bucket.
- `bucket_arn`: The Amazon Resource Name (ARN) of the bucket.

---

## Notes

- This module includes a bucket policy that grants AWS CloudTrail permissions to write logs securely.
- The log prefix used is: `AWSLogs/<account-id>/`
- Versioning ensures that deleted or overwritten logs can still be recovered within the lifecycle window.
- Lifecycle management helps optimize storage cost by purging stale and unused log versions.
