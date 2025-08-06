# VPC Peering Terraform Module

This Terraform submodule provisions a **VPC Peering Connection** between two VPCs, including route configuration for both the requester and accepter sides. It supports both **intra-region** and **cross-region** peering and provides routing updates for public and private subnets on both ends.

---

## Features

- Creates a VPC peering connection between a local VPC and a peer VPC
- Supports automatic or manual peering acceptance
- Updates public and private route tables for both requester and accepter sides
- Supports cross-region VPC peering
- Flexible and environment-agnostic

---

## Usage

```hcl
module "vpc_peering" {
  source = "github.com/kmkouokam/infra-modules//aws/modules/vpc_peering"

  vpc_id                       = "vpc-0abc1234"
  peer_vpc_id                  = "vpc-0def5678"
  peer_aws_region              = "us-west-2"
  auto_accept                  = true
  vpc_cidr                     = "10.0.0.0/16"
  peer_vpc_cidr                = "10.1.0.0/16"
  public_route_table_ids       = ["rtb-0aaa", "rtb-0aab"]
  private_route_table_ids      = ["rtb-0aac"]
  peer_public_route_table_ids  = ["rtb-0dda"]
  peer_private_route_table_ids = ["rtb-0ddb"]
  env                          = "prod"
}
```

---

## Input Variables

- `vpc_id`: The VPC ID of the local (requester) VPC.
- `peer_vpc_id`: The VPC ID of the peer (accepter) VPC.
- `peer_aws_region`: The AWS region where the peer VPC resides.
- `auto_accept`: Whether to automatically accept the peering request (default: `false`).
- `vpc_cidr`: The CIDR block of the local VPC.
- `peer_vpc_cidr`: The CIDR block of the peer VPC.
- `public_route_table_ids`: List of public route table IDs for the local VPC.
- `private_route_table_ids`: List of private route table IDs for the local VPC.
- `peer_public_route_table_ids`: List of public route table IDs for the peer VPC.
- `peer_private_route_table_ids`: List of private route table IDs for the peer VPC.
- `env`: The environment label to apply to resources (e.g., `dev`, `prod`).

---

## Outputs

- `aws_vpc_peering_connection_id`: The ID of the created VPC peering connection.

---

## Notes

- The CIDR blocks of both VPCs must **not overlap**.
- Cross-region VPC peering requires additional IAM permission if accounts differ.
- Ensure route table entries are configured correctly to allow traffic between peered VPCs.
- This module does not create any security group rules. You must allow traffic using security groups or network ACLs.
 