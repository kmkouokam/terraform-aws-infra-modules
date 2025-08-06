 # Bastion Host Terraform Module

This module provisions a secure EC2-based Bastion Host in a public subnet for SSH access to private resources in your VPC.

---

## Features

- Launches a Bastion EC2 instance in a specified public subnet.
- Uses the latest Ubuntu 24.04 AMI from SSM Parameter Store.
- Associates a public IP for SSH access.
- Accepts custom SSH key, IAM instance profile, and security groups.
- Accepts tags and environment naming.
- Supports initialization with a `user_data.sh` script.

---

## Inputs

- `aws_region` – AWS region to deploy the Bastion host.
- `env` – Environment name (e.g., `dev`, `prod`) used for naming and tagging.
- `bastion_instance_type` – EC2 instance type for the Bastion (e.g., `t3.micro`).
- `public_subnet_ids` – List of public subnet IDs (only the first is used).
- `security_group_ids` – Security Group IDs to attach to the Bastion instance.
- `key_name` – SSH key pair name used for login access.
- `allowed_ssh_cidrs` – List of allowed CIDR blocks for SSH access (not directly used in this module unless you handle it in your SG).
- `iam_instance_profile_name` – Name of the IAM instance profile to attach.
- `tags` – Key-value map of tags to apply to the instance and resources.

---

## Outputs

- `bastion_instance_id` – The ID of the created Bastion EC2 instance.
- `public_subnet_ids` – The public IP address assigned to the Bastion host.

---

## Example Usage

```hcl
module "bastion" {
  source                    = "github.com/kmkouokam/infra-modules//aws/modules/bastion"
  aws_region                = "us-east-1"
  env                       = "prod"
  bastion_instance_type     = "t3.micro"
  public_subnet_ids         = aws_subnet.public_subnets[*].id
  security_group_ids        = ["sg-abc123"]
  key_name                  = "my-key"
  allowed_ssh_cidrs         = ["0.0.0.0/0"]
  iam_instance_profile_name = module.iam_roles.ec2_instance_profile_name
  tags = {
    Project     = "MyApp"
    Environment = var.env
  }
}
