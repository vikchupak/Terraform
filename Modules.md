Group logically related resources in modules

- https://registry.terraform.io/browse/modules
- https://gitlab.com/twn-devops-bootcamp/latest/12-terraform/terraform-learn/-/tree/feature/modules

# Local modules from `./modules` folder

- These are your own modules, stored inside your project folder.
- You reference them with a local source path.

```hcl
module "vpc" {
  source = "./modules/vpc"
  cidr_block = "10.0.0.0/16"
}
```

Module files
- `main.tf` - Module’s main logic.
- `providers.tf` (**only for root module** - best proctice to keep only on the root module. This is because provider settings (e.g., credentials, regions, or endpoints) are global to the Terraform configuration.
- `outputs.tf` (like exports for modules) outputs from the root module just print variables, NOT exports.  Outputs from child modules doesn't print, BUT export.
- `variables.tf` (like arguments) Variables are passed from the root module to child modules.
- `terraform.tfvars` (**only for the root module** and then the root passes the vars to its child modules)

---

- `root module` is root folder's main.tf
- `child modules` are modules in modules folder and called from other configurations

---

Import child module to root module

```hcl
module "myapp-subnet" {
  # define module source
  source = "./modules/subnet"

  # pass variable values to the child module from the root module variables
  subnet_cidr_block = var.subnet_cidr_block
  env_prefix = var.env_prefix
  vpc_id = aws_vpc.myapp-vpc.id

  # pass variable values to the child module as hardcoded value
  avail_zone = "eu-central-1b"

  # pass variable values to the child module from the root module resource
  default_route_table_id = aws_vpc.myapp-vpc.default_route_table_id
}
```

Access child module resources from root module
```hcl
subnet_id = module.myapp-subnet.subnet.id

# In child module must be such output for this to work
output "subnet" {
  value = aws-subnet.myapp-subnet-1
}
```

# Public modules installed from Terraform Registry

- These are published modules from Terraform Registry.
- You reference them with a remote source URL.

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "~> 5.0"
  cidr    = "10.0.0.0/16"
}
```

- We can 'import' and use the modules inside main.tf file
- Or we can organize things better to split modules into separate `.tf` files and keep them in the root folder
  - Terraform will still auto-load all `.tf` files in the folder. No extra "import" needed.
