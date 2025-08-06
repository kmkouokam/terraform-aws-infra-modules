 # Jenkins EC2 Submodule

This Terraform submodule provisions a Jenkins server on an EC2 instance with IAM roles and policies for S3 access, using Ubuntu 24.04. It sets up the necessary networking, security, and user data bootstrap configuration to install and run Jenkins in your specified environment.

---

## Features

- Launches a Jenkins EC2 instance in a public subnet.
- Uses the latest Canonical Ubuntu 24.04 AMI via SSM.
- Configures IAM roles and policies to allow Jenkins access to a specified S3 bucket for CI/CD artifact storage.
- Automatically installs Jenkins using a user data script (`install_jenkins.sh`).
- Tags and names all resources with the provided `env` value for traceability.

---

## Usage

```hcl
module "jenkins" {
  source             = "github.com/kmkouokam/infra-modules//aws/modules/jenkins"
  env                = var.env
  instance_type      = "t3.medium"
  key_name           = "my-key-pair"
  bucket_name        =  var.bucket_name
  public_subnet_ids  = aws_subnet.public_subnets[*].id
  security_group_ids = [aws_security_group.jenkins_sg.id]
}
```

Ensure your `install_jenkins.sh` script is located in the module directory and contains the commands to install Jenkins, configure ports, and start the service.

---

## Inputs

- `env`: The environment label (e.g., `dev`, `prod`) used in resource naming.
- `instance_type`: The EC2 instance type to launch (e.g., `t3.medium`).
- `key_name`: The name of the SSH key pair for access.
- `bucket_name`: The S3 bucket name used for storing Jenkins build artifacts.
- `public_subnet_ids`: A list of public subnet IDs to host the EC2 instance.
- `security_group_ids`: A list of security group IDs to associate with the EC2 instance.

---

## Outputs

This module does not define explicit outputs, but you may extend it to output values such as the Jenkins instance's public IP address, DNS name, or IAM role ARN.

---

## Notes

- This module assumes that Jenkins will be installed via `install_jenkins.sh`, which should be tailored to your OS and Jenkins setup.
- The EC2 instance is automatically associated with a public IP address for web access.
- Ensure the associated security group allows inbound access on port 8080 (Jenkins default UI port) and port 22 (SSH).
- Jenkins can be configured to use the specified S3 bucket for build artifact storage via plugins or CLI.
