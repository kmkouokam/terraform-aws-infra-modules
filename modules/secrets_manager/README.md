# Secrets Manager Module

This Terraform submodule provisions an AWS Secrets Manager secret for securely storing sensitive values, such as database credentials. It generates a strong random password, stores it in a structured JSON format, and encrypts it using a provided KMS key.

> This module is intended to be used as part of the `kmkouokam/infra-modules/aws` repository.

---

## Features

- Generates a random strong password
- Creates a versioned secret in AWS Secrets Manager
- Encrypts the secret with a KMS key
- Appends a timestamp suffix to the secret name to ensure uniqueness
- Returns the stored secret version for reference

---

## Example Usage

```hcl
module "secrets_manager" {
  source      = "github.com/kmkouokam/infra-modules//aws/modules/secrets_manager"

  env         = "prod"
  secret_name = "mysql-password"
  description = "MySQL database root password"
  kms_key_id  =  module.kms.secrets_manager_kms_key_id
}
```

---

## Input Variables

- `env`: The environment name (e.g., dev, staging, prod)
- `secret_name`: The base name of the secret
- `description`: A short description of the secret's purpose
- `kms_key_id`: The ARN of the KMS key to use for encryption

---

## Outputs

- `secret_name`: The unique ID of the created secret
- `secret_arn`: The ARN of the created secret
- `secret_password`: The generated random password (retrieve password from secret)
- `password`: The generated random password 



---

## Notes

- The password is stored in the secret as a JSON object: `{ "password": "your-password" }`
- A timestamp is added to the secret name for traceability and uniqueness
- You can retrieve the secret later using `aws_secretsmanager_secret_version`

---

## Related Docs

- [Terraform: aws_secretsmanager_secret](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/secretsmanager_secret)
- [Terraform: random_password](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/password)
 