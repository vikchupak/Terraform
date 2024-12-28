- A source is the location where to fetch data from.
- A resource is the actual infrastructure object created.

In Terraform, providers are plugins that enable interaction with APIs of various services, platforms, or systems. They are essential because they manage the resources defined in Terraform configuration files. Providers are responsible for understanding API interactions and exposing resources through Terraform.

Providers are configured in Terraform using the provider block.
Default terraform repository `hashicorp`. Example for AWS:
```hcl
provider "aws" {
  region = "us-east-1"
}
```

Custom repositories must be specified explicitly
```hcl
provider "linode" {
  source = "linode/linode"
  version = "2.9.5"
}
```

After providers defined ve have to install them:
```bash
# Where this command is run from is important
terraform init
```
