# AWS Cost Optimization Module

This Terraform module implements **cost management and optimization strategies** on AWS using:

- **Cost Categories** for Environment tagging
- **Anomaly Detection** with daily alerts
- **Monthly Budgets** with customizable thresholds and email notifications

---

## 📦 Resources Provisioned

### 🧮 `aws_ce_cost_category.env_category`
- Creates a **Cost Category** for billing reports.
- Groups linked account usage under:
  - `Production`
  - `Development`

> Cost categorization helps track and optimize spend across environments.

---

### 📊 `aws_ce_anomaly_monitor.ec2_monitor`
- Sets up an anomaly monitor scoped to the **SERVICE** dimension (`EC2` in this case).
- Detects unusual cost spikes on EC2 services.

### 📧 `aws_ce_anomaly_subscription.email_alert`
- Sends **daily email alerts** for detected cost anomalies.
- Threshold for alerting: `$100+` absolute anomaly impact.
- Supports **multiple subscriber emails**.

---

### 💰 `aws_budgets_budget.monthly_budget`
- Defines a **monthly cost budget** (amount passed as a variable).
- Filters for:
  - Service: `Amazon EC2`
- Triggers alerts when spending reaches **90% of the monthly limit**.
- Sends email notifications (SNS optional but currently unused).
- Tags the budget with the current environment name.

---

## 🔧 Input Variables

| Name           | Description                                 | Type          | Required |
|----------------|---------------------------------------------|---------------|----------|
| `env`          | Environment name (e.g., `dev`, `prod`)      | `string`      | ✅ Yes    |
| `budget_amount`| Monthly budget amount in USD                | `string`      | ✅ Yes    |
| `alert_email`  | List of email addresses for alerts          | `list(string)`| ✅ Yes    |

---

## 🚀 Example Usage

```hcl
module "cost_optimization" {
  source         = "github.com/kmkouokam/infra-modules//aws/modules/cost_optimization"
  env            = var.env
  budget_amount  = "500"
  alert_email    = ["alerts@example.com", "finance@example.com"]
}
```

---

## ✅ Features

- ✅ **Cost Categories** for tagging usage by environment
- ✅ **Anomaly detection** for EC2 costs with threshold-based alerts
- ✅ **Budgeting with email notifications**
- ✅ **Daily monitoring and real-time visibility**
- ✅ Scalable for multiple environments or accounts

---

## 🛠️ Notes

- This module uses your **linked account ID** via `data.aws_caller_identity`.
- Email subscribers will receive AWS confirmation emails for anomaly alerts.
- SNS topic ARNs can be integrated later if needed for automation workflows.

---

## 📄 License

This project is licensed under the **Mozilla Public License 2.0** (MPL-2.0).  
See the [LICENSE](./LICENSE) file for full details.
