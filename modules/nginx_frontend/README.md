# Nginx Frontend Infrastructure Module

This Terraform module provisions a scalable, high-availability Nginx frontend architecture on AWS using the following components:

- Ubuntu 24.04 AMI (fetched dynamically)
- EC2 Launch Template with Nginx user data script
- Auto Scaling Group (ASG)
- Application Load Balancer (ALB)
- Target Group for HTTP traffic
- Customizable parameters for instance type, key pair, subnets, and security groups

---

## 🧱 Components Deployed

### 📦 AMI Data Source
- **Source:** AWS Systems Manager (SSM)
- **AMI:** Ubuntu Server 24.04 (Canonical, HVM, EBS, gp3)
- **Fetched via:** `data.aws_ssm_parameter.ubuntu_ami`

---

### 🚀 Launch Template
- **Resource:** `aws_launch_template.nginx`
- **Key Features:**
  - Ubuntu 24.04 AMI
  - Instance type defined via variable
  - User data script for Nginx installation (`install_nginx.sh`)
  - IAM instance profile and security group support

---

### 🏗️ Auto Scaling Group
- **Resource:** `aws_autoscaling_group.nginx_asg`
- **Configuration:**
  - Launch template-based scaling
  - Custom min/max/desired capacity
  - Attached to public subnets
  - Integrated with target group for ALB

---

### 🌐 Application Load Balancer (ALB)
- **Resource:** `aws_lb.nginx_alb`
- **Configuration:**
  - Public ALB
  - Supports HTTP traffic on port 80
  - Attached to public subnets and security groups

---

### 🎯 Target Group
- **Resource:** `aws_lb_target_group.nginx_tg`
- **Purpose:** Routes ALB traffic to EC2 instances
- **Health Checks:**
  - Path: `/`
  - Interval: 30 seconds
  - Healthy threshold: 2
  - Unhealthy threshold: 2

---

## 🔧 Required Variables

| Name                        | Description                                | Type     |
|-----------------------------|--------------------------------------------|----------|
| `env`                       | Environment name prefix                     | `string` |
| `vpc_id`                    | ID of the VPC                               | `string` |
| `public_subnet_ids`         | List of public subnet IDs                   | `list`   |
| `elb_security_group_ids`    | List of security group IDs for the ALB      | `list`   |
| `nginx_security_group_ids`  | List of security group IDs for EC2          | `list`   |
| `key_name`                  | Name of the EC2 key pair                    | `string` |
| `iam_instance_profile_name`| IAM instance profile name for EC2           | `string` |
| `forntend_instance_type`    | EC2 instance type (e.g., `t3.micro`)        | `string` |
| `desired_capacity`          | Desired number of EC2 instances             | `number` |
| `min_size`                  | Minimum number of EC2 instances             | `number` |
| `max_size`                  | Maximum number of EC2 instances             | `number` |

> **Note:** The user data script `install_nginx.sh` must exist in the same directory or module path.

---

## 🚀 Example Usage

```hcl
module "nginx_frontend" {
  source                    = "./modules/nginx_frontend"
  env                       = "dev"
  vpc_id                    = "vpc-xxxxxxxx"
  public_subnet_ids         = aws_subnet.public_subnets[*].id
  elb_security_group_ids    = ["sg-xxxxxx"]
  nginx_security_group_ids  = ["sg-yyyyyy"]
  key_name                  = "my-keypair"
  iam_instance_profile_name = module.iam_roles.ec2_instance_profile_name
  forntend_instance_type    = "t3.micro"
  desired_capacity          = 2
  min_size                  = 1
  max_size                  = 3
}
```

---

## ✅ Requirements

- Terraform v1.0+
- AWS Provider v4.0+
- Ubuntu 24.04 compatible AMI via SSM

---

## 📄 License

This project is licensed under the **Mozilla Public License 2.0** (MPL-2.0).  
See the [LICENSE](./LICENSE) file for full details.
