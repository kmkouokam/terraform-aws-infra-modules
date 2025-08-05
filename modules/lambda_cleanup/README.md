# AWS Lambda Cleanup Module

This Terraform module provisions a scheduled AWS Lambda function that automatically cleans up unused EC2 resources and publishes notifications to SNS.

---

## 🚀 What This Module Does

- Creates an **IAM Role** with permissions to:
  - Describe and terminate EC2 instances
  - Describe and release Elastic IP addresses
  - Describe and delete EBS volumes
  - Write logs to CloudWatch Logs
  - Publish messages to a specified SNS topic
- Deploys a **Python 3.11 Lambda function** (`CleanupUnusedResources`) from a packaged zip file
- Schedules a **weekly CloudWatch Event rule** to trigger the Lambda every Monday at 00:00 UTC
- Grants CloudWatch Events permission to invoke the Lambda function

---

## 📦 Resources Created

| Resource                          | Purpose                                        |
|----------------------------------|------------------------------------------------|
| `aws_iam_role.lambda_cleanup_role`        | IAM role assumed by the Lambda function        |
| `aws_iam_policy.lambda_cleanup_policy`    | Policy granting EC2, Logs, and SNS permissions |
| `aws_iam_role_policy_attachment.attach_cleanup_policy` | Attaches cleanup policy to Lambda role          |
| `aws_iam_policy.lambda_sns_publish`       | Additional SNS publish policy                   |
| `aws_iam_role_policy_attachment.lambda_sns_attachment` | Attaches SNS policy to Lambda role               |
| `aws_lambda_function.cleanup`               | The cleanup Lambda function deployed            |
| `aws_cloudwatch_event_rule.weekly_cleanup` | CloudWatch scheduled rule triggering Lambda    |
| `aws_cloudwatch_event_target.cleanup_target` | Event target linking the rule to Lambda         |
| `aws_lambda_permission.allow_cloudwatch`   | Permission for CloudWatch to invoke Lambda      |

---

## 🔧 Input Variables

| Name              | Description                                | Type   | Required |
|-------------------|--------------------------------------------|--------|----------|
| `aws_sns_topic_arn`| ARN of the SNS topic to publish notifications | `string` | ✅ Yes    |
| `env`             | (Optional) Environment name for tagging or naming | `string` | No       |

---

## 📋 Usage Example

```hcl
module "lambda_cleanup" {
  source            = "./modules/lambda_cleanup"
  aws_sns_topic_arn = aws_sns_topic.alerts.arn
  env               = var.env
}
```

---

## 🛠️ Notes

- The Lambda function code must be packaged as `function.zip` and placed in the module directory.
- The function runs weekly on Mondays at midnight UTC by default (modifiable via the cron expression).
- The Lambda environment variables include `LOG_LEVEL` and `SNS_TOPIC_ARN` for logging and notifications.
- Make sure the SNS topic exists and the Lambda has permissions to publish to it.

---

## 📄 License

This project is licensed under the **Mozilla Public License 2.0** (MPL-2.0).  
See the [LICENSE](./LICENSE) file for details.
