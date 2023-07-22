---
description: Complimentary configuration to support Advanced AWS installation
---

# AWS Networking

The standard networking CloudFormation template deploys the following resources:

* a VPC with CIDR range `10.0.0.0/16`
* 2 public subnets with CIDR ranges `10.0.0.0/24` and `10.0.1.0/24`
* 2 private subnets with CIDR ranges `10.0.2.0/24` and `10.0.3.0/24`
* an Internet Gateway
* a NAT Gateway
* a public and private route table
* all required attachments and associations

This is not required to run sorry-cypress but is provided as a convenience when deploying the [advanced template](advanced-aws-setup.md). Any pre-existing VPC with at least 1 subnet and routing to the Internet can be used.
