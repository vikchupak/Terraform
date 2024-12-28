- A `source` is the location where to fetch data from (repository).

Providers attributes:
- A `resource` is the actual infrastructure object created.
  - https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ami
- A `data` (data source) is a way to fetch metadata about existing resources.

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
# terraform-vpc is a name we give the resource inside the terraform
resource "aws_vpc" "terraform-vpc" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "terraform-subnet" {
  vpc_id = aws_vpc.terraform-vpc.id
  cidr_block = "10.0.10.0/24"
  availability_zone = "eu-central-1a"
}

# Get existing data
data "aws_vpc" "existing_vpc" {
  default = true
}

# Use existing data
resource "aws_subnet" "terraform-subnet-2" {
  vpc_id = data.aws_vpc.existing_vpc.id
  cidr_block = "172.32.48.0/20"
  availability_zone = "eu-central-1a"
}
```

After providers defined we have to install them:
```bash
# Where this command is run from is important
terraform init
```

Create defined resources
```bash
terraform apply
```
