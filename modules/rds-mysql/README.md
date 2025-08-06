 # RDS MySQL Module

This Terraform submodule provisions one or more Amazon RDS MySQL instances using a dynamic `for_each` map. It sets up associated subnet groups and configures essential options like encryption, backup retention, and availability zones.

> This module is intended to be used as part of the `kmkouokam/infra-modules/aws` repository.

---

## Features

- Provisions multiple RDS instances using a dynamic map
- Creates RDS subnet groups for proper AZ distribution
- Enables storage encryption with a KMS key
- Supports backup retention, AZ targeting, and versioning
- Adds security group support and instance tagging

---

## Example Usage

```hcl
module "rds_mysql" {
  source = "github.com/kmkouokam/infra-modules//aws/modules/rds-mysql"

  engine_version      = "8.0"
  instance_class      = "db.t3.micro"
  storage_size        = 20
  kms_key_id          = module.kms.rds_kms_key_arn
  security_group_ids  = [aws_security_group.rds_sg.id, aws_security_group.jenkins_sg.id, aws_security_group.bastion_sg.id, aws_security_group.nginx_sg.id] 

  rds_instances = {
    app1 = {
      db_name            = "app1db"
      db_username        = "admin"
      db_password        = "securepass1"
      private_subnet_ids = ["subnet-aaa", "subnet-bbb"]
      az_name            = "us-east-1a"
    },
    app2 = {
      db_name            = "app2db"
      db_username        = "admin"
      db_password        = "securepass2"
      private_subnet_ids = ["subnet-ccc", "subnet-ddd"]
      az_name            = "us-east-1b"
    }
  }
}
```

---

## Input Variables

- `rds_instances`: A map of instances, each containing:
  - `db_name`
  - `db_username`
  - `db_password`
  - `private_subnet_ids`
  - `az_name`
- `engine_version`: MySQL engine version (e.g., `"8.0"`)
- `instance_class`: EC2 instance class for the database (e.g., `"db.t3.micro"`)
- `storage_size`: Allocated storage (in GB)
- `security_group_ids`: List of security group IDs
- `kms_key_id`: ARN of the KMS key for encryption

---

## Outputs

- `rds_endpoints`: A map of database names to their corresponding RDS connection endpoints
- `rds_instance_identifiers`: A map of instance keys to their unique RDS identifiers
- `rds_instance_ids`: A map of instance keys to the actual AWS resource IDs of the DB instances

---

## Notes

- `skip_final_snapshot` is enabled by default
- Multi-AZ is set to `false` (you can modify as needed)
- Storage type is set to `gp2` (general purpose SSD)

---

## Related Terraform Docs

- [aws_db_instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/db_instance)
- [aws_db_subnet_group](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/db_subnet_group)
