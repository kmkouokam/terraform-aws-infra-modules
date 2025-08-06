 # NGINX Frontend Terraform Module

This Terraform module provisions a scalable NGINX-based frontend architecture using EC2, Auto Scaling Group (ASG), Launch Template, and Application Load Balancer (ALB). It’s ideal for deploying public-facing web services in a highly available setup.

---

## Features

- Launches EC2 instances with Ubuntu 24.04 and installs NGINX using a user data script
- Creates a Launch Template for consistent instance configuration
- Deploys an Application Load Balancer (ALB) with health checks
- Configures an Auto Scaling Group (ASG) with min, max, and desired capacity
- Assigns CloudWatch agent and X-Ray instance profile roles (optional)
- Supports configurable IAM, SSH access, security groups, and networking

---

## Inputs

- `env`: Environment name (e.g., `dev`, `prod`)
- `forntend_instance_type`: EC2 instance type (e.g., `t3.micro`)
- `iam_instance_profile_name`: IAM instance profile name for NGINX EC2 instances
- `xray_instance_profile_name`: IAM instance profile name for AWS X-Ray (optional)
- `cloudwatch_agent_role_name`: Name of the IAM role for CloudWatch Agent
- `key_name`: SSH key pair name for accessing instances
- `public_subnet_ids`: List of public subnet IDs to launch EC2 instances in
- `vpc_id`: The VPC ID to associate with the frontend resources
- `elb_security_group_ids`: Security groups for the ALB
- `nginx_security_group_ids`: Security groups for the NGINX instances
- `desired_capacity`: Desired number of instances in the ASG
- `min_size`: Minimum number of instances in the ASG
- `max_size`: Maximum number of instances in the ASG

---

## Outputs

- `frontend_instance_id`: ID(s) of the created Launch Template(s) for NGINX
- `frontend_instance_name`: Name(s) of the NGINX launch template(s)
- `nginx_alb_name`: Name of the Application Load Balancer
- `nginx_alb_arn`: ARN of the Application Load Balancer

---

## Example Usage

```hcl
module "nginx_frontend" {
  source = "github.com/kmkouokam/infra-modules//aws/modules/nginx_frontend"

  env                        = "prod"
  vpc_id                     = "vpc-abc123"
  public_subnet_ids          = ["subnet-123", "subnet-456"]
  forntend_instance_type     = "t3.micro"
  key_name                   = "my-key"
  iam_instance_profile_name  = "nginx-ec2-profile"
  cloudwatch_agent_role_name = "cw-agent-role"
  xray_instance_profile_name = "xray-profile"
  nginx_security_group_ids   = ["sg-nginx"]
  elb_security_group_ids     = ["sg-alb"]
  desired_capacity           = 2
  min_size                   = 1
  max_size                   = 4
}
```

---

## Notes

- The module expects a `install_nginx.sh` script for configuring NGINX to exist in the same module directory.
- Ensure security groups allow inbound HTTP (port 80) traffic and health checks.
- The ALB and ASG are deployed in public subnets only.
