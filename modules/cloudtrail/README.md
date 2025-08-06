 # CloudTrail Submodule

This Terraform submodule sets up AWS CloudTrail to record account activity across all AWS services and regions. It delivers logs to a specified S3 bucket, enables log file integrity validation, and includes support for data events, specifically S3 object-level actions.

---

## Features

- Multi-region trail that captures all management events and S3 data events.
- Enables log file validation for added integrity and security.
- Supports custom tagging for resource organization.
- Designed for centralized logging and compliance auditing.
- Outputs the trail name and ARN for downstream use.

---

## Usage

```hcl
module "cloudtrail" {
  source           = "github.com/kmkouokam/infra-modules//aws/modules/cloudtrail"
  env              = var.en
  s3_bucket_name   = "my-cloudtrail-logs-bucket-name"
  tags = {
    Project     = "my-project-name"
    Environment = var.env
  }
}
```

---

## Inputs

- `env`: Environment name (e.g., `dev`, `prod`). Used for naming the CloudTrail instance.
- `s3_bucket_name`: Name of the S3 bucket where CloudTrail logs will be delivered.
- `tags`: A map of tags to assign to the CloudTrail resource.

---

## Outputs

- `trail_name`: The name of the created CloudTrail.
- `trail_arn`: The ARN of the created CloudTrail.

---

## Notes

- This module captures both management and S3 data events.
- You must ensure the provided S3 bucket exists and has the correct permissions to receive CloudTrail logs.
- The module is intended for multi-region visibility and should be deployed once per AWS account for best practices.
