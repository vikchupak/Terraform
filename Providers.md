- A source is the location where to fetch data from.
- A resource is the actual infrastructure object created.
  - https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ami

In Terraform, providers are plugins that enable interaction with APIs of various services, platforms, or systems. They are essential because they manage the resources defined in Terraform configuration files. Providers are responsible for understanding API interactions and exposing resources through Terraform.

Providers are configured in Terraform using the provider block.
Default terraform repository `hashicorp`.
Custom repositories must be specified explicitly.
```hcl
# Configure provider sources
terraform {
  required_providers {
    # AWS can be skiped, but it is a good practice to add it explicitly
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
    linode = {
      source = "linode/linode"
      version = "2.9.5"
    }
  }
}

# Configure the AWS Provider
provider "aws" {
  region = "us-east-1"
}

# Configure the linode Provider
provider "linode" {
  # configuration
}

# Create a VPC
resource "aws_vpc" "example" {
  cidr_block = "10.0.0.0/16"
}
```

After providers defined we have to install them:
```bash
# Where this command is run from is important
terraform init
```
