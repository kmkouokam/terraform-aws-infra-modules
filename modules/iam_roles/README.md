# IAM Roles & Policies Module

This Terraform module provisions various IAM roles, policies, and instance profiles required for AWS services including EC2, RDS, WAF, CloudTrail, CloudWatch, and AWS X-Ray. It is designed to follow security and monitoring best practices for your AWS infrastructure.

---

## 📁 Resources Provisioned

### 🔐 EC2 IAM Role
- **Purpose:** Grants EC2 instances access to AWS Systems Manager (SSM) and CloudWatch monitoring.
- **Role:** `${var.env}-ec2-role`
- **Attached Policies:**
  - `AmazonSSMManagedInstanceCore`
  - `CloudWatchAgentServerPolicy`
  - Custom inline policy for CloudWatch and X-Ray actions

### 📈 CloudWatch Agent Role
- **Purpose:** Allows CloudWatch Agent on EC2 to collect and send metrics/logs.
- **Role:** `${var.env}-cw-agent-role`
- **Policy:** `CloudWatchAgentServerPolicy`

### 🛢️ RDS Monitoring Role
- **Purpose:** Enables enhanced monitoring for RDS MySQL.
- **Role:** `${var.env}-rds-monitoring-role`
- **Policy:** `AmazonRDSEnhancedMonitoringRole`

### 🛡️ WAF Logging Role
- **Purpose:** Allows AWS WAF to log events to CloudWatch Logs.
- **Role:** `${var.env}-waf-logging-role`
- **Inline Policy:** Custom policy for CloudWatch Logs actions

### 📜 CloudTrail Logging Role
- **Purpose:** Allows CloudTrail to publish logs to CloudWatch.
- **Role:** `${var.env}-cloudtrail-logs-role`
- **Inline Policy:** Custom policy for CloudWatch Logs actions

### 📊 CloudWatch Monitoring Permissions
- **Inline Policy:** Grants EC2 permission to:
  - List and get CloudWatch metrics
  - Send traces to AWS X-Ray

### 📦 X-Ray EC2 Role
- **Purpose:** Allows EC2 instances to send trace data to AWS X-Ray.
- **Role:** `xray-ec2-role`
- **Policy:** `AWSXRayDaemonWriteAccess`

---

## 👤 IAM Instance Profiles

Instance profiles created for EC2 instances to assume the above roles:

| Instance Profile Name | Associated Role |
|------------------------|-----------------|
| `${var.env}-ec2-instance-profile` | `ec2_role` |
| `${var.env}-cw-agent-instance-profile` | `cw_agent_role` |
| `${var.env}-cw-agent-instance-profile-${random_id}` | `cw_agent_role` |
| `xray-ec2-profile` | `xray_ec2_role` |

---

## 📌 Usage

```hcl
module "iam_roles" {
  source = "./modules/iam_roles"  # Update path as needed
  env    = "dev"
}
```

---

## 🔧 Variables

| Name | Description | Type | Required |
|------|-------------|------|----------|
| `env` | Environment name used for naming resources (e.g., `dev`, `prod`) | `string` | ✅ Yes |

---

## ✅ Requirements

- Terraform v1.0+
- AWS Provider v4.0+

---

## 🔐 Security Best Practices

- Follows least-privilege principle for IAM roles and policies.
- Isolates roles for each AWS service to ensure secure role assumptions.
- Supports enhanced observability via CloudWatch and AWS X-Ray.

---

## 📄 License

This project is licensed under the **Mozilla Public License 2.0** (MPL-2.0).  
See the [LICENSE](./LICENSE) file for details.