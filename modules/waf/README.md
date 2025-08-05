# AWS WAFv2 Web ACL Module

This Terraform module provisions an **AWS WAFv2 Web ACL** with logging and CloudWatch metrics enabled, using AWS-managed rule groups. It also associates the WAF with an **Application Load Balancer (ALB)** or a **CloudFront distribution**, depending on the chosen scope.

---

## 📦 Resources Provisioned

### 🔐 `aws_wafv2_web_acl.waf-metrics`
- **Purpose:** Creates a Web ACL for the specified environment and scope (`REGIONAL` or `CLOUDFRONT`)
- **Default Action:** Allow all (customize if needed)
- **Managed Rule Group:** `AWSManagedRulesCommonRuleSet`
- **Metrics:** Enabled via CloudWatch
- **Sampled Requests:** Enabled
- **Tags:** Provided via input variable

---

### 🔗 `aws_wafv2_web_acl_association.waf_to_alb`
- **Purpose:** Associates the WAF Web ACL with an ALB (or other compatible resource)
- **Resource ARN:** Provided via input (e.g., ALB ARN)
- **Lifecycle:** `create_before_destroy` ensures safe updates

---

### 📊 `aws_wafv2_web_acl_logging_configuration.waf_logging`
- **Purpose:** Sends WAF logs to a specified CloudWatch Log Group
- **Redacted Fields:**
  - HTTP Method
  - URI Path
- **Log Group ARN:** Provided via variable `waf_logging_group_arn`

---

## 🔧 Input Variables

| Name                  | Description                                                   | Type     | Required |
|-----------------------|---------------------------------------------------------------|----------|----------|
| `env`                 | Environment name prefix (e.g., `dev`, `prod`)                 | `string` | ✅ Yes    |
| `scope`               | WAF scope: `"REGIONAL"` for ALB, `"CLOUDFRONT"` for CloudFront| `string` | ✅ Yes    |
| `tags`                | Tags to apply to WAF resources                                | `map`    | ✅ Yes    |
| `nginx_alb_arn`       | ARN of the Application Load Balancer to associate with WAF    | `string` | ✅ Yes    |
| `waf_logging_group_arn` | ARN of the CloudWatch Log Group for WAF logging             | `string` | ✅ Yes    |

---

## 🚀 Example Usage

```hcl
module "waf" {
  source = "github.com/kmkouokam/infra-modules//aws/modules/waf"

  env                   = "your env"
  scope                 = "REGIONAL"
  tags                  = {
    Environment = "your env"
    Team        = "security"
  }
  nginx_alb_arn         = module.nginx_frontend.nginx_alb_arn
  waf_logging_group_arn = module.cloudwatch.waf_logging_group_arn
}
```

---

## ✅ Features

- ✅ AWS-managed rules (Common Rule Set)
- ✅ CloudWatch metrics and sampling
- ✅ WAF logs sent to CloudWatch Logs
- ✅ Safe resource lifecycle handling
- ✅ Modular and reusable across environments

---

## 📄 License

This project is licensed under the **Mozilla Public License 2.0** (MPL-2.0).  
See the [LICENSE](./LICENSE) file for full details.
