# AWS CloudWatch Monitoring Module

This Terraform module provisions AWS CloudWatch resources for monitoring various AWS services, including **EC2**, **RDS**, **Lambda**, and **WAF**. It includes dashboards, metric alarms, and log group configurations to help you gain operational visibility and trigger alerts based on performance thresholds.

---

## 📊 Features

- 📈 **CloudWatch Dashboard** for:
  - EC2 cleanup activity metrics
  - Lambda function execution statistics
- 🚨 **CloudWatch Alarms** for:
  - High CPU usage on RDS and EC2
  - Low memory on RDS
  - High disk utilization on EC2
- 📁 **CloudWatch Log Group** for:
  - AWS WAF logging

---

## 📁 Files

- `main.tf`: Core Terraform code defining the CloudWatch resources.
- `variables.tf`: Input variables used in the module (not shown here).
- `outputs.tf`: Outputs from the module (if defined).
- `README.md`: Documentation.

---

## 📥 Inputs

| Name                     | Description                                              | Type        | Required |
|--------------------------|----------------------------------------------------------|-------------|----------|
| `env`                    | Environment name used for naming and tagging             | `string`    | ✅ Yes    |
| `aws_region`             | AWS region for dashboard and alarms                      | `string`    | ✅ Yes    |
| `lambda_function_name`   | Name of the Lambda function to monitor                   | `string`    | ✅ Yes    |
| `rds_instance_names`     | Map of RDS instance names to monitor                     | `map(string)` | ✅ Yes  |
| `frontend_instance_name` | List of EC2 instance IDs (e.g., frontend) to monitor     | `list(string)` | ✅ Yes |
| `aws_sns_topic_arn`      | SNS topic ARN for alarm notifications                    | `string`    | ✅ Yes    |

---

## 📤 Outputs (Optional)

Outputs can include:

- Dashboard name
- Alarm names
- WAF log group name

---

## ✅ Example Usage

```hcl
module "cloudwatch_monitoring" {
  source                  = "./modules/cloudwatch"
  env                     = "you-env"
  aws_region              = "us-east-1"
  lambda_function_name    = "ec2-cleanup-function"
  rds_instance_names      =  module.rds_mysql.rds_instance_identifiers
  frontend_instance_name  =  module.nginx_frontend.frontend_instance_name[*]
  aws_sns_topic_arn       = aws_sns_topic.alerts.arn
}
