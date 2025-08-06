# CloudWatch Monitoring Module

This Terraform module provisions AWS CloudWatch resources to enable monitoring and alerting for EC2, RDS, Lambda, and AWS WAF. It creates dashboards, alarms, and log groups for effective observability of your infrastructure.

---

## Features

- Creates a CloudWatch dashboard with EC2 cleanup and Lambda metrics.
- Provisions alarms for:
  - RDS CPU and memory usage
  - EC2 frontend CPU and disk utilization
- Creates a CloudWatch log group for AWS WAF metrics.
- Sends alerts to an SNS topic.
- Uses dynamic inputs for environment, region, and instance names.

---

## Input Variables

- `aws_region`: AWS region where the resources will be created.
- `env`: Environment name (e.g., dev, prod) used to prefix resource names.
- `iam_instance_profile_name`: IAM instance profile for EC2.
- `cloudwatch_agent_role_name`: IAM role for CloudWatch agent.
- `cloudwatch_agent_profile_name`: Instance profile for CloudWatch agent.
- `lambda_function_name`: Lambda function monitored in the dashboard.
- `frontend_instance_name`: List of EC2 instance IDs (frontend).
- `rds_instance_names`: Map of RDS instance names.
- `aws_sns_topic_arn`: SNS topic ARN for sending CloudWatch alarm notifications.

---

## Output

- `waf_logging_group_arn`: ARN of the CloudWatch log group created for AWS WAF logs.

---

## Example Usage

```hcl
module "cloudwatch" {
  source                        = "github.com/kmkouokam/infra-modules//aws/modules/cloudwatch"
  env                           = var.env
  aws_region                    = "us-east-1"
  iam_instance_profile_name     = module.iam_roles.cloudwatch_agent_profile_name
  cloudwatch_agent_role_name    = module.iam_roles.cloudwatch_agent_role_name
  cloudwatch_agent_profile_name =  module.iam_roles.cloudwatch_agent_profile_name
  lambda_function_name          = module.lambda_cleanup.lambda_function_name
  frontend_instance_name        = module.nginx_frontend.frontend_instance_name[*]
  rds_instance_names            = module.rds_mysql.rds_instance_identifiers
  aws_sns_topic_arn             = aws_sns_topic.alerts.arn
}
 