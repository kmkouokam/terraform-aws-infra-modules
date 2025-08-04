AWS Cost Management Infrastructure
This Terraform configuration provisions a fully-featured AWS infrastructure optimized for cost management, high availability, and observability.
________________________________________
🔍 Overview
This infrastructure includes a VPC with subnets across availability zones, NAT gateways, EC2 instances, cost monitoring, and a set of modules designed for scalability, security, and operational insight.
Developed by
Ernestine D. Motouom
kmkouokam@yahoo.com
________________________________________
🚀 Components Provisioned
🔹 VPC Networking
•	VPC CIDR: 10.0.0.0/16
•	Internet Gateway
•	2 Public Subnets: 10.0.1.0/24, 10.0.2.0/24
•	2 Private Subnets: 10.0.3.0/24, 10.0.4.0/24
•	NAT Gateways: 2 (1 per public subnet)
🔹 EC2 Instances (Compute)
•	Bastion Host for SSH
•	NGINX Frontend behind ELB with autoscaling
•	Jenkins instance for CI/CD
🔹 RDS (Database)
•	MySQL engine 8.0
•	Multi-AZ deployment
•	Encrypted with KMS
🔹 VPC Peering
•	Cross-region peering with a peer VPC in us-east-2
•	Routing setup for communication between VPCs
🔹 Security
•	IAM Roles and policies
•	KMS Encryption (Secrets, RDS, etc.)
•	WAF for application-layer protection
•	Security groups scoped for each resource
🔹 Observability & Monitoring
•	CloudWatch Dashboards, Metrics, Alarms
•	CloudTrail for auditing
•	SNS topic for alarm notifications
•	Logs stored in S3
🔹 Automation
•	Lambda function for unused resource cleanup
•	CloudWatch agent for log and metric collection
🔹 Secrets Management
•	AWS Secrets Manager for storing RDS credentials
•	Encrypted with KMS
🔹 VPN Setup
•	VPN module using customer gateway and VPN connection
•	Scoped security group rules for ports 80, 443, and 22
________________________________________
📦 Modules Used
Each module resides under the modules/ directory:
Module	Purpose
vpc_peering/	Cross-region VPC peering
rds-mysql/	RDS MySQL instances (multi-AZ)
bastion/	Bastion host with SSH key
nginx_frontend/	NGINX web server behind ELB
jenkins/	Jenkins EC2 for CI/CD
iam_roles/	IAM policies and instance profiles
kms/	KMS keys for encryption
secrets_manager/	Store and retrieve secrets securely
cloudtrail/	Setup CloudTrail
cloudwatch/	CloudWatch agent, alarms, dashboards
waf/	AWS WAF rule management
vpn/	VPN configuration
lambda_cleanup/	Lambda for EBS/EIP cleanup
s3_logs/	Central S3 bucket for logs
cost_optimization/	Budget alerts and cleanup logic
________________________________________
🔧 Setup
Prerequisites
•	Terraform CLI >= 1.4
•	AWS CLI
•	IAM user with admin or equivalent permissions
Apply Infrastructure
terraform init
terraform plan -out=tfplan
terraform apply tfplan
________________________________________
📬 Notifications
•	SNS topics send alerts to:
o	nycarine0@gmail.com
o	kmkouokam@yahoo.com
________________________________________
🧠 Notes
•	Ensure your key pair keypair exists in AWS EC2.
•	Cost optimization Lambda is scheduled for cleanup of:
o	Unattached EBS volumes
o	Unused EIPs
•	Modify variables.tf to adapt to your environment.
 
