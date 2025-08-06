# AWS Modular Infrastructure with Cost Management

This Terraform project provisions a fully modular, multi-AZ AWS environment with a focus on **cost optimization**, **security**, and **scalability**.  
Developed by **Ernestine D. Motouom**.

---

## Module Structure

This root module orchestrates the following submodules:

- modules/iam_roles – IAM roles and instance profiles  
- modules/kms – KMS keys for encryption  
- modules/secrets_manager – Secure secrets storage  
- modules/rds-mysql – RDS MySQL (multi-AZ, encrypted)  
- modules/bastion – Bastion EC2 instance in public subnet  
- modules/nginx_frontend – NGINX frontend with ALB and auto scaling  
- modules/cloudwatch – Metrics, alarms, and dashboards  
- modules/cloudtrail – Audit logging  
- modules/s3_logs – S3 bucket for logs and artifacts  
- modules/waf – AWS WAF with CloudWatch logging  
- modules/vpn – VPN gateway and subnet integration  
- modules/vpc_peering – Cross-region VPC peering  
- modules/lambda_cleanup – Cleanup Lambda for unused EC2 resources  
- modules/cost_optimization – Budgets, alerts, and RI suggestions  
- modules/jenkins – Jenkins setup in a public subnet  

---

## Key Components

This infrastructure includes the following:

- Custom VPC and peer VPC (multi-region: `us-east-1`, `us-east-2`)  
- Public and private subnets across Availability Zones  
- Internet Gateways and NAT Gateways  
- Bastion Host with SSH access  
- Jenkins server deployed in public subnet  
- NGINX frontend with ALB and autoscaling  
- RDS MySQL instances (Multi-AZ) with secure access  
- S3 Bucket for logs and Jenkins artifacts  
- CloudTrail with centralized logging  
- CloudWatch metrics, alarms, and dashboards  
- Lambda function for EC2 cleanup automation  
- AWS WAF with logging enabled  
- VPN with secure subnet routing  
- VPC peering between primary and peer VPCs  
- IAM roles and instance profiles  
- Secrets Manager with KMS encryption  
- Cost optimization module with budgets and alerts  

---

## Features

- Multi-AZ and Region: High availability through AZ distribution and peer VPC in another region  
- Security Best Practices: IAM roles, encrypted storage (KMS), WAF, and strict security group rules  
- Automation: EC2 cleanup via Lambda, custom dashboards and alarms via CloudWatch  
- Cost Awareness: Integrated budget monitoring and reserved instance strategies  
- Logging & Auditing: Centralized logging using S3 and CloudTrail  

---

## Variables

### Environment
- env (string): Deployment environment. Default: `"production"`

### Networking
- aws_region (string): AWS region to deploy resources. Default: `"us-east-1"`  
- vpc_cidr (string): CIDR block for the VPC. Default: `"10.0.0.0/16"`  
- public_subnet_cidrs (list): CIDR blocks for public subnets. Default: `["10.0.1.0/24", "10.0.2.0/24"]`  
- private_subnet_cidrs (list): CIDR blocks for private subnets. Default: `["10.0.3.0/24", "10.0.4.0/24"]`  
- ipv6_cidr_block (string): IPv6 CIDR block for inbound rules. Default: `"::/0"`  

### VPC Peering
- peer_aws_region (string): Region of the peer VPC. Default: `"us-east-2"`  
- peer_vpc_cidr (string): CIDR block for peer VPC. Default: `"10.1.0.0/16"`  
- peer_public_subnet_cidrs (list): CIDRs for peer VPC public subnets. Default: `["10.1.1.0/24", "10.1.2.0/24"]`  
- peer_private_subnet_cidrs (list): CIDRs for peer VPC private subnets. Default: `["10.1.3.0/24", "10.1.4.0/24"]`  
- peer_public_subnet_ids (list): IDs for peer public subnets. Default: `["10.1.1.0/24", "10.1.2.0/24"]`  
- peer_private_subnet_ids (list): IDs for peer private subnets. Default: `["10.1.3.0/24", "10.1.4.0/24"]`  
- auto_accept (bool): Whether to auto-accept the peering request. Default: `false`  

### Compute
- bastion_instance_type (string): Instance type for Bastion host. Default: `"t2.micro"`  
- frontend_instance_type (string): Instance type for frontend EC2. Default: `"t2.micro"`  
- jenkins_instance_type (string): Instance type for Jenkins EC2. Default: `"t2.micro"`  
- key_name (string): SSH key name for EC2 access. Default: `"virg.keypair"`  
- allowed_ssh_cidrs (list): CIDRs allowed to SSH into the Bastion host. Default: `["0.0.0.0/0"]`  

### Auto Scaling
- desired_capacity (number): Desired number of frontend instances. Default: `2`  
- min_size (number): Minimum number of frontend instances. Default: `1`  
- max_size (number): Maximum number of frontend instances. Default: `3`  

### Security
- secret_name (string): Name of the secret in Secrets Manager. Default: `"my_secret"`  
- description (string): Description for the secret. Default: `"Managed secret"`  
- kms_key_id (string): KMS key ID to encrypt the secret. Default: `"alias/aws/secretsmanager"`  
- multi_az (bool): Enable multi-AZ deployment for RDS. Default: `false`  

### Alerts and Monitoring
- notification_emails (list): Emails for SNS alert subscriptions.  
  Default: `["nycarine0@gmail.com", "kmkouokam@yahoo.com"]`  

### Tagging
- tags (map): Common tags applied to all resources. Default:  
  {
    Owner       = "Ernestine D Motouom"
    Project     = "refonte_class"
    Environment = "Production"
  }

---

## Outputs

- vpc_id – ID of the created VPC  
- public_subnet_ids – List of public subnet IDs  
- private_subnet_ids – List of private subnet IDs  
- secret_name – Name of the created Secrets Manager secret  
- jenkins_security_group – Security group ID for Jenkins  
- kms_key_id – KMS key ID used for Secrets Manager  
- rds_kms_key_id – KMS key ID used for RDS  
- s3_kms_key_id – KMS key ID used for S3  
- secrets_manager_kms_key_id – Same as kms_key_id  
- rds_sg_id – Security group ID for RDS  
- rds_instance_ids – Map of RDS instance IDs  
- rds_endpoints – Map of RDS instance endpoints  
- bastion_sg_id – Security group ID for Bastion host  
- elb_security_group_ids – Security group ID for ALB/ELB  
- nginx_security_group_ids – Security group ID for NGINX frontend  
- vpn_security_group_ids – Security group ID for VPN  
- public_route_table_ids – IDs of route tables for public subnets  
- private_route_table_ids – IDs of route tables for private subnets  
- peer_public_route_table_ids – Route tables for peer public subnets  
- peer_private_route_table_ids – Route tables for peer private subnets  
- aws_sns_topic_alerts_arn – ARN of the SNS topic for alerts  

---

## Deployment Requirements

- Terraform CLI v1.0 or higher  
- AWS CLI configured with admin privileges  
- Remote state backend (recommended)  
- AWS provider >= 4.0  

---

## Usage

To deploy this module:

```bash
terraform init
terraform plan -out=tfplan
terraform apply tfplan
```

---

## License

This project is licensed under the [Mozilla Public License 2.0](https://www.mozilla.org/en-US/MPL/2.0/).  
See the [LICENSE](./LICENSE) file for full details.