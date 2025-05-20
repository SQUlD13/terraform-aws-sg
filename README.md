# Terraform Security Group Module

This Terraform module creates an AWS Security Group that:

- Allows **SSH access** only from a specified local IP.
- Allows **HTTP (port 80)** and **HTTPS (port 443)** access from **anywhere**.
- Allows **all outbound traffic**.

## Usage

```hcl
module "security_group" {
  source   = "squid13/sg/aws"
  version = "1.0.0"
  local_ip = var.local_ip
}
```
### Input
| Name      | Description                           | Type   | Default | Required |
| --------- | ------------------------------------- | ------ | ------- | -------- |
| local\_ip | The IP address allowed for SSH access | string | n/a     | yes      |

You can pass your local IP dynamically with something like:

```bash
export TF_VAR_local_ip=$(curl -s https://checkip.amazonaws.com)/32
```

Or hardcode it:

```hcl
local_ip = "203.0.113.5/32"
```

### Output
| Name                | Description                          |
| ------------------- | ------------------------------------ |
| security\_group\_id | The ID of the created security group |


# Requirements

- AWS credentials must be configured (via environment, config file, or shared credentials).

- Terraform >= 1.0

Providers

    aws

Example

To use this module with an EC2 instance:

```hcl
module "security_group" {
  source   = "squid13/sg/aws"
  version = "1.0.0"
  local_ip = var.local_ip
}

module "ec2_instance" {
  source             = "./modules/ec2_instance"
  security_group_id  = module.security_group.security_group_id
}
```