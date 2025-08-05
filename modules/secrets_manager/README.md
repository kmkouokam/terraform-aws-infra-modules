# AWS Secrets Manager - Random Password Generator Module

This Terraform module securely generates and stores a **random password** in **AWS Secrets Manager**, encrypted with a KMS key.

---

## 🔐 What This Module Does

- Generates a **strong, 12-character random password** (letters, numbers, and limited special characters)
- Creates a **Secrets Manager secret** with a unique timestamp-based name
- Stores the password in JSON format under the key `password`
- Encrypts the secret using a specified **KMS key**
- Optionally retrieves the stored secret version for reference/use in other modules

---

## 📦 Resources Created

| Resource                                        | Purpose                                      |
|------------------------------------------------|----------------------------------------------|
| `random_password.password`                     | Generates a secure, random password          |
| `aws_secretsmanager_secret.db_secret`          | Creates the Secrets Manager secret           |
| `aws_secretsmanager_secret_version.db_secret_version` | Stores the password as a secret version       |
| `data.aws_secretsmanager_secret_version.retrieved` | Optionally retrieves the stored secret version |

---

## 🔧 Input Variables

| Name           | Description                              | Type     | Required |
|----------------|------------------------------------------|----------|----------|
| `secret_name`  | Base name for the secret (timestamp will be appended) | `string` | ✅ Yes    |
| `description`  | Description of the secret                 | `string` | No       |
| `kms_key_id`   | KMS Key ID used for encryption            | `string` | ✅ Yes    |
| `env`          | Environment name used in tags             | `string` | ✅ Yes    |

---

## 📋 Example Usage

```hcl
module "secrets" {
  source       = "github.com/kmkouokam/infra-modules//aws/modules/secrets"
  secret_name  = var.secret_name
  description  = "Password for MySQL RDS instance"
  kms_key_id   = module.kms.secrets_manager_kms_key_id
  env          = var.env
}
```

## 🛡️ Security Notes

- Password is generated using Terraform’s `random_password` resource.
- Only safe special characters are used (no risky symbols like quotes or backslashes).
- Secret is encrypted using customer-managed KMS key.
- Each secret name includes a timestamp to ensure uniqueness and traceability.

---

## 📄 License

This project is licensed under the **Mozilla Public License 2.0** (MPL-2.0).  
See the [LICENSE](./LICENSE) file for details.
