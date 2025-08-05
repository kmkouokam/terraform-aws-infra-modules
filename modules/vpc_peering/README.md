# VPC Peering Connection Module

This Terraform module establishes a **VPC Peering Connection** between two AWS VPCs (requester and accepter) across the same or different regions. It also configures routing on both sides (public and private route tables) to enable full network communication.

---

## 🔁 Resources Provisioned

### 🔷 VPC Peering Connection (Requester)
- **Resource:** `aws_vpc_peering_connection.peer`
- **Establishes** a peering request from the local (requester) VPC to the remote (accepter) VPC.
- **Supports cross-region** peering via `peer_region`
- **Tags:** `"Side" = "Requester"`

---

### 🔁 (Optional) VPC Peering Connection Accepter
> ❗ Not currently used, but can be enabled if auto-accept is not configured or cross-account peering is required.
<!-- Uncomment in `main.tf` to use `aws_vpc_peering_connection_accepter.peer` -->

---

### 📡 Routing Configuration

#### 🟦 Requester Side Routes

| Resource | Description |
|----------|-------------|
| `aws_route.requester_public_routes`  | Adds routes to the peer VPC in each **public** route table in the requester VPC |
| `aws_route.requester_private_routes` | Adds routes to the peer VPC in each **private** route table in the requester VPC |

#### 🟩 Accepter Side Routes

| Resource | Description |
|----------|-------------|
| `aws_route.accepter_public_routes`  | Adds routes to the requester VPC in each **public** route table in the accepter VPC |
| `aws_route.accepter_private_routes` | Adds routes to the requester VPC in each **private** route table in the accepter VPC |

---

## 📌 Required Variables

| Name                        | Description                                 | Type     |
|-----------------------------|---------------------------------------------|----------|
| `vpc_id`                    | ID of the **requester** VPC                 | `string` |
| `vpc_cidr`                  | CIDR block of the **requester** VPC         | `string` |
| `peer_vpc_id`              | ID of the **accepter** VPC                  | `string` |
| `peer_vpc_cidr`            | CIDR block of the **accepter** VPC          | `string` |
| `peer_aws_region`          | AWS region of the peer VPC                  | `string` |
| `auto_accept`              | Whether to auto-accept the peering request  | `bool`   |
| `public_route_table_ids`   | List of public route table IDs (requester)  | `list`   |
| `private_route_table_ids`  | List of private route table IDs (requester) | `list`   |
| `peer_public_route_table_ids`   | List of public route table IDs (accepter)  | `list`   |
| `peer_private_route_table_ids`  | List of private route table IDs (accepter) | `list`   |

---

## 🚀 Example Usage

```hcl
module "vpc_peering" {
  source = "./modules/vpc_peering"   #update as needed

  vpc_id                      = "vpc-012345"
  vpc_cidr                    = "10.0.0.0/16"
  peer_vpc_id                = "vpc-678910"
  peer_vpc_cidr              = "10.1.0.0/16"
  peer_aws_region            = "us-west-2"
  auto_accept                = true

  public_route_table_ids         = ["rtb-aaaaaa"]
  private_route_table_ids        = ["rtb-bbbbbb"]
  peer_public_route_table_ids    = ["rtb-cccccc"]
  peer_private_route_table_ids   = ["rtb-dddddd"]
}
```

---

## ✅ Features

- ✅ Cross-region peering support
- ✅ Automatic or manual peering acceptance
- ✅ Routing for both VPCs (public and private)
- ✅ Modular and reusable

---

## 📄 License

This project is licensed under the **Mozilla Public License 2.0** (MPL-2.0).  
See the [LICENSE](./LICENSE) file for full details.
