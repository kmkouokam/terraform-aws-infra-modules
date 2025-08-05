# 🌐 AWS Client VPN Terraform Module

This Terraform module provisions an **AWS Client VPN Endpoint** and associates it with public and private subnets across availability zones. It uses certificate-based authentication and ACM for secure access.

---

## 🚀 Features

- Creates a **Client VPN Endpoint**
- Uses **ACM-issued certificates** for mutual TLS (mTLS) authentication
- Associates VPN endpoint with **public and private subnets**
- Configures **routing** to allow VPN traffic through the subnets
- Adds **authorization rules** to permit access to VPC networks

---

## 📁 Resources Created

| Resource Type                                 | Description                                 |
|----------------------------------------------|---------------------------------------------|
| `aws_ec2_client_vpn_endpoint`                | Creates the VPN server endpoint             |
| `aws_ec2_client_vpn_network_association`     | Associates the VPN with selected subnets    |
| `aws_ec2_client_vpn_route`                   | Routes VPN traffic to VPC subnets           |
| `aws_ec2_client_vpn_authorization_rule`      | Grants access to VPC CIDR block             |
| `data.aws_acm_certificate`                   | Retrieves ACM certificate for VPN           |
| `data.aws_subnet`                            | Looks up subnet details                     |

---

## 🔧 Input Variables

| Name                    | Description                                                  | Type        | Required |
|-------------------------|--------------------------------------------------------------|-------------|----------|
| `env`                   | Environment name (e.g., dev, prod)                            | `string`    | ✅ Yes   |
| `vpc_id`                | VPC ID to associate with the VPN endpoint                    | `string`    | ✅ Yes   |
| `vpc_cidr_block`        | CIDR block of the VPC to authorize in VPN                    | `string`    | ✅ Yes   |
| `public_subnet_ids`     | List of public subnet IDs                                    | `list(string)` | ✅ Yes |
| `private_subnet_ids`    | List of private subnet IDs                                   | `list(string)` | ✅ Yes |
| `vpn_aws_security_group`| Security group ID for the VPN endpoint                       | `string`    | ✅ Yes   |
| `tags`                  | Key-value tags to apply to resources                         | `map(string)` | No     |

---

## 🔐 Certificate Authentication

This module uses **ACM-issued TLS certificates** for:
- The **server certificate** (`*.devopsguru.world`)
- The **root certificate chain** for client authentication

Make sure:
- The ACM certificate is already **issued and validated**
- You manage client certificates appropriately

---

## 🗺️ Routing

The module sets up:
- One **VPN route per unique subnet** (both public and private)
- Default route (`0.0.0.0/0`) targeting each associated subnet

---

## ✅ Example Usage

```hcl
module "vpn" {
  source                = "./modules/vpn"
  env                   = var.env
  vpc_id                = "vpc-1234567890abcdef"
  vpc_cidr_block        = "10.0.0.0/16"
  public_subnet_ids     = ["subnet-abc1", "subnet-abc2"]
  private_subnet_ids    = ["subnet-def1", "subnet-def2"]
  vpn_aws_security_group = "sg-0abc12345"
  tags = {
    Project = "VPN Access"
    Owner   = "prod-team"
  }
}
```

---

## ⚠️ Notes

- Ensure the ACM certificate exists and is in **Issued** status.
- Only **one subnet per AZ** is associated with the VPN (as required by AWS).
- Route authorization is set to allow all groups. You may adjust this for fine-grained control.

---

## 📄 License

This project is licensed under the **Mozilla Public License 2.0**.

---

 