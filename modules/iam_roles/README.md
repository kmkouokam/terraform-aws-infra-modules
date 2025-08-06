 # IAM Roles Terraform Module

This Terraform module creates IAM roles, policies, and instance profiles to support AWS workloads including EC2, CloudWatch, RDS monitoring, WAF logging, CloudTrail logging, and AWS X-Ray.

---

## Features

- EC2 IAM role with permissions for SSM and CloudWatch agent  
- CloudWatch agent role and policies  
- RDS MySQL Enhanced Monitoring role  
- WAF logging role with CloudWatch Logs permissions  
- CloudTrail logging role with CloudWatch Logs permissions  
- AWS X-Ray EC2 role with daemon write access  
- Instance profiles for associating roles to EC2 instances  
- Environment-based naming convention for resources  

---

## Inputs

- `env`: The deployment environment, used as a prefix for IAM roles and instance profiles (string, required)

---

## Outputs

- `rds_monitoring_role_arn`: ARN of the RDS monitoring IAM role  
- `waf_logging_role_arn`: ARN of the WAF logging IAM role  
- `cloudtrail_logs_role_arn`: ARN of the CloudTrail logs IAM role  
- `ec2_instance_profile_name`: Name of the EC2 instance profile with SSM and CloudWatch permissions  
- `cloudwatch_agent_role_name`: Name of the CloudWatch agent IAM role  
- `cloudwatch_agent_profile_name`: Name of the CloudWatch agent instance profile  
- `ec2_cloudwatch_metrics_name`: Name of the IAM policy for EC2 CloudWatch metrics  
- `xray_instance_profile_name`: Name of the instance profile for AWS X-Ray EC2 role  

---

## Usage Example

```hcl
module "iam_roles" {
  source = "github.com/kmkouokam/infra-modules//aws/modules/iam_roles"
  env    = var.env
}

 
