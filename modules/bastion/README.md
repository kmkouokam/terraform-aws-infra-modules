# Bastion Host Terraform Module

This Terraform module provisions a **Bastion Host EC2 instance** on AWS. The bastion host is a secure entry point used to connect to private instances in your Virtual Private Cloud (VPC).

---

## 🚀 Features

- Launches an EC2 instance using the latest Ubuntu 24.04 AMI (via SSM).
- Attaches to a public subnet with a public IP for SSH access.
- Associates IAM instance profile and SSH key.
- Uses user data to bootstrap the host on launch.
- Tags the instance with environment-specific labels.

---

## 📁 Files

- `main.tf`: Core Terraform configuration for the bastion instance.
- `user_data.sh`: Optional shell script for configuring the instance at launch (base64-encoded).
- `variables.tf`: Expected input variables (not shown here).
- `outputs.tf`: Outputs such as the instance ID, public IP (expected, if defined).

---

## 📥 Inputs

| Name                      | Description                                         | Type     | Required |
|---------------------------|-----------------------------------------------------|----------|----------|
| `bastion_instance_type`   | EC2 instance type (e.g., `t3.micro`)                | `string` | ✅ Yes    |
| `public_subnet_ids`       | List of public subnet IDs                           | `list`   | ✅ Yes    |
| `security_group_ids`      | List of security group IDs to attach                | `list`   | ✅ Yes    |
| `key_name`                | Name of the SSH key pair                            | `string` | ✅ Yes    |
| `iam_instance_profile_name` | Name of the IAM instance profile                 | `string` | ✅ Yes    |
| `env`                     | Environment tag (e.g., `dev`, `prod`)               | `string` | ✅ Yes    |

---

## 📤 Outputs (Optional)

If defined in `outputs.tf`, you can expect:

- `bastion_instance_id`
- `bastion_public_ip`

---

## 📘 Example Usage

```hcl
module "bastion" {
  source                    = "./modules/bastion"
  bastion_instance_type     = var.bastion_instance_type
  public_subnet_ids         = aws_subnet.public_subnets[*].id
  security_group_ids        = [aws_security_group.bastion_sg.id]
  key_name                  = "my-key-pair"
  iam_instance_profile_name = module.iam_roles.ec2_instance_profile_name
  env                       = "your-env"
}
