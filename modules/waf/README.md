 # WAF Submodule

This Terraform module provisions an AWS WAFv2 Web ACL and associates it with an Application Load Balancer (ALB) or CloudFront distribution. It includes logging, managed rule sets, and monitoring via CloudWatch metrics.

---

## Features

- Creates a WAFv2 Web ACL with the `AWSManagedRulesCommonRuleSet`.
- Associates the WAF with an ALB or CloudFront distribution.
- Enables CloudWatch metrics and logging for visibility and monitoring.
- Redacts sensitive fields (HTTP method and URI path) from logs.
- Supports customizable environment tagging and scope selection.

---

## Usage

```hcl
module "waf" {
  source = "github.com/kmkouokam/infra-modules//aws/modules/waf"

  env                   = var.env
  scope                 = "REGIONAL"
  tags                  = { Project = "my-app", Owner = "team" }
  waf_logging_group_arn = module.cloudwatch.waf_logging_group_arn
  nginx_alb_arn         = module.nginx_frontend.nginx_alb_arnabc123"
  waf_logging_role_arn  = module.iam_roles.waf_logging_role_arn
}
```

---

## Inputs

- `env`: Name of the environment (e.g., `dev`, `prod`). Used in resource naming and tagging.
- `scope`: The scope of the WAF. Valid values are `"REGIONAL"` for ALB or `"CLOUDFRONT"` for global distributions.
- `tags`: A map of key-value tags to apply to WAF resources.
- `waf_logging_group_arn`: ARN of the CloudWatch Log Group where WAF logs will be sent.
- `nginx_alb_arn`: ARN of the ALB to associate with the WAF Web ACL.
- `waf_logging_role_arn`: IAM role ARN that allows WAF to write to CloudWatch Logs (currently declared but not used in this module).

---

## Notes

- This module uses AWS-managed rule sets to block common threats.
- CloudWatch logging includes sampled requests, metrics, and redacts sensitive fields.
- You must manually create the logging IAM role if required.
- For `CLOUDFRONT` scope, ensure the WAF is deployed in the `us-east-1` region as required by AWS.

