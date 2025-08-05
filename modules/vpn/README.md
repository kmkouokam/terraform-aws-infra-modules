# VPN Submodule for AWS Infrastructure

This is a reusable **Terraform submodule** used within the `kmkouokam/infra-modules/aws` module. It provisions an **AWS Client VPN Endpoint** with certificate-based authentication, subnet associations, and routing configuration.

## ❗ Note

Although this module is designed for internal use within `kmkouokam/infra-modules/aws`, it can be reused independently if desired. However, **direct use is recommended only if you understand its dependencies and input requirements**.

## 📦 What it Does

- Provisions an AWS Client VPN Endpoint
- Uses ACM-issued TLS certificates
- Associates public and private subnets
- Configures routing for VPN users
- Authorizes full access to the VPC CIDR block

## 🔧 Usage Example

```hcl
module "vpn" {
  source                 = "github.com/kmkouokam/infra-modules//aws/modules/vpn"
  env                    = "dev"
  vpc_id                 = "vpc-xxxxxxxx"
  vpc_cidr_block         = "10.0.0.0/16"
  public_subnet_ids      = ["subnet-abc123", "subnet-def456"]
  private_subnet_ids     = ["subnet-ghi789", "subnet-jkl012"]
  vpn_aws_security_group = "sg-xxxxxxx"
  tags = {
    Environment = "dev"
    Owner       = "devops"
  }
}
```

## 📥 Inputs

See `variables.tf` for all configurable options, including:

- `env`
- `vpc_id`
- `vpc_cidr_block`
- `public_subnet_ids`
- `private_subnet_ids`
- `vpn_aws_security_group`
- `tags`

## 📤 Outputs

See `outputs.tf` (if defined) for any exposed outputs such as:

- `vpn_endpoint_id`
 

## 📄 License

This module is licensed under the **Mozilla Public License 2.0**.

## 📚 Learn More

- [Terraform Registry: Module Structure](https://developer.hashicorp.com/terraform/registry/modules/publishing#standard-module-structure)
- [AWS Client VPN Documentation](https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/what-is.html)
 