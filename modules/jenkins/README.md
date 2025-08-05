# Jenkins Server Module

This Terraform module provisions a **Jenkins CI server** on AWS EC2 with the necessary IAM roles, permissions, and network settings.

---

## 🚀 What This Module Does

- Creates an **IAM Role** for the Jenkins EC2 instance with permissions to access S3 buckets for artifacts.
- Creates an **IAM Instance Profile** linked to the Jenkins EC2 role.
- Retrieves the latest **Ubuntu 24.04 AMI** from AWS Systems Manager Parameter Store.
- Launches an **EC2 instance** running Ubuntu with:
  - Configured instance type and subnet
  - Assigned security groups and SSH key
  - Attached IAM instance profile
  - User data script to install and configure Jenkins (`install_jenkins.sh`)

---

## 📦 Resources Created

| Resource                      | Purpose                                           |
|-------------------------------|--------------------------------------------------|
| `aws_iam_role.jenkins_ec2_role`            | EC2 assume role for Jenkins instance               |
| `aws_iam_role_policy.jenkins_ci_permissions` | IAM policy granting S3 read/write/list access for artifacts |
| `aws_iam_instance_profile.jenkins_instance_profile` | Instance profile attached to the EC2 instance        |
| `aws_instance.jenkins_server`              | EC2 instance running Jenkins with Ubuntu AMI        |

---

## 🔧 Input Variables

| Name                | Description                                    | Type        | Required |
|---------------------|------------------------------------------------|-------------|----------|
| `env`               | Environment prefix (e.g., `dev`, `prod`)      | `string`    | ✅ Yes    |
| `bucket_name`        | S3 bucket name for storing Jenkins artifacts   | `string`    | ✅ Yes    |
| `instance_type`      | EC2 instance type for Jenkins server           | `string`    | ✅ Yes    |
| `public_subnet_ids`  | List of public subnet IDs to launch the server | `list(string)` | ✅ Yes    |
| `security_group_ids` | List of security group IDs attached to instance| `list(string)` | ✅ Yes    |
| `key_name`           | SSH key pair name for EC2 instance access      | `string`    | ✅ Yes    |

---

## 📋 Usage Example

```hcl
module "jenkins" {
  source            = "github.com/kmkouokam/infra-modules//aws/modules/jenkins"
  env               = var.env
  bucket_name       = var.bucket_name
  instance_type     = "t3.medium"
  public_subnet_ids = ["subnet-0123456789abcdef0", "subnet-0fedcba9876543210"]
  security_group_ids = ["sg-0a1b2c3d4e5f6g7h8"]
  key_name          = "my-ssh-key"
}
```

---

## 🔐 IAM Permissions

- Jenkins EC2 instance role is granted:
  - `s3:GetObject`
  - `s3:PutObject`
  - `s3:ListBucket`
  
  on the specified S3 bucket for storing build artifacts.

---

## 🖥️ Jenkins Installation

- The `install_jenkins.sh` script (in the module folder) runs during EC2 instance initialization.
- It installs and configures Jenkins automatically on Ubuntu 24.04 LTS.

---

## 🛠️ Notes

- Ensure the specified security groups allow inbound access for Jenkins (default port 8080) and SSH (port 22).
- The instance is launched with a public IP; adjust if deploying in private subnets or behind a NAT.
- Modify `install_jenkins.sh` as needed to customize Jenkins setup.

---

## 📄 License

This project is licensed under the **Mozilla Public License 2.0** (MPL-2.0).  
See the [LICENSE](./LICENSE) file for details.
