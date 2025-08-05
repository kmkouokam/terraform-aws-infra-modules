# AWS RDS MySQL Instances Module

This Terraform module provisions one or more **AWS RDS MySQL instances** along with their associated subnet groups.

---

## 🚀 What This Module Does

- Creates an **RDS Subnet Group** per database instance using specified private subnets.
- Provisions **MySQL RDS instances** with configurable:
  - Engine version
  - Instance class (e.g., db.t3.medium)
  - Storage size and type (gp2)
  - Multi-AZ deployment (currently set to single AZ)
  - Database name, username, and password per instance
  - Availability zone selection per instance
  - Security groups attachment
  - Backup retention (7 days)
  - Encryption with specified KMS key
  - Timeout configurations for creation and updates
- Tags resources with instance-specific names.

---

## 🔧 Input Variables

| Name               | Description                                  | Type                  | Required |
|--------------------|----------------------------------------------|-----------------------|----------|
| `rds_instances`    | Map of RDS instance configurations. Each key is an instance name, value is an object with:<br>- `private_subnet_ids` (list of subnet IDs)<br>- `db_name`<br>- `db_username`<br>- `db_password`<br>- `az_name` (availability zone) | `map(object)`         | ✅ Yes    |
| `engine_version`   | MySQL engine version to use (e.g., `"8.0"`) | `string`              | ✅ Yes    |
| `instance_class`   | RDS instance class (e.g., `"db.t3.medium"`) | `string`              | ✅ Yes    |
| `storage_size`     | Allocated storage size in GB                  | `number`              | ✅ Yes    |
| `security_group_ids`| List of security group IDs for the RDS instances | `list(string)`         | ✅ Yes    |
| `kms_key_id`       | KMS Key ID to use for storage encryption      | `string`              | ✅ Yes    |

---

## 📋 Example Usage

```hcl


module "rds_mysql" {
  source            = "github.com/kmkouokam/infra-modules//aws/modules/rds_mysql"
  rds_instances     = var.rds_instances
  engine_version    = "8.0"
  instance_class    = "db.t3.micro"
  storage_size      = 20
  security_group_ids = [aws_security_group.rds_sg.id, aws_security_group.jenkins_sg.id, aws_security_group.bastion_sg.id, aws_security_group.nginx_sg.id] 
  kms_key_id        = module.kms.rds_kms_key_arn
}
```

---

## 🛠️ Notes

- The module creates a separate DB subnet group for each RDS instance.
- `multi_az` is currently disabled; enable if you want high availability.
- `skip_final_snapshot` is set to true — **be careful** with data loss on deletion.
- `storage_encrypted` is enabled and uses the provided KMS key.
- Adjust timeout values as needed based on your environment.

---

## 📄 License

This project is licensed under the **Mozilla Public License 2.0** (MPL-2.0).  
See the [LICENSE](./LICENSE) file for details.
