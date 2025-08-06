 # Cost Optimization Submodule

This Terraform submodule configures AWS cost optimization features to help monitor, control, and manage cloud expenses across environments. It includes AWS Budgets, Cost Categories, and Anomaly Detection. The module is ideal for environments requiring active cost tracking, alerting, and categorization based on usage.

---

## Features

- **Cost Categories**: Organizes AWS spend into categories such as `Production` and `Development`, using the current account ID.
- **Anomaly Detection**: Detects unusual spikes in EC2-related spending with a daily evaluation cycle.
- **Email Alerts**: Sends cost anomaly and budget threshold notifications to a specified list of email recipients.
- **Budgets**: Enforces a monthly cost limit and sends alerts at 90% usage.

---

## Usage

Include this module in your Terraform configuration to enable cost monitoring:

```hcl
module "cost_optimization" {
  source         = "github.com/kmkouokam/infra-modules//aws/modules/cost_optimization"
  env            = var.env
  budget_amount  = 300
  alert_email    = ["finops@example.com", "admin@example.com"]
}
```

---

## Inputs

- `env`: (Required) Name of the environment (e.g., `dev`, `prod`). Used for tagging resources and identifying context.
- `budget_amount`: (Optional) Monthly AWS budget in USD. Defaults to `100`.
- `alert_email`: (Optional) A list of email addresses to receive alerts for cost anomalies and budget breaches. Defaults to two preset emails.

---

## Notes

- Anomaly detection is configured for the `EC2` service only. You can extend this to other services as needed.
- Alerts are sent daily and triggered based on a cost anomaly threshold (set to $100).
- Budget notifications are triggered at 90% of the defined limit.
- No SNS topics are attached by default, but you can customize this within the budget configuration if needed.

There are no outputs defined for this module. All resources are managed internally for cost optimization purposes.
