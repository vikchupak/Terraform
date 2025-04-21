Group logically related resources in modules

- https://registry.terraform.io/browse/modules
- https://gitlab.com/twn-devops-bootcamp/latest/12-terraform/terraform-learn/-/tree/feature/modules

Module files
- `main.tf` - Module’s main logic.
- `providers.tf` (**only for root module** - best proctice to keep only on the root module. This is because provider settings (e.g., credentials, regions, or endpoints) are global to the Terraform configuration.
- `outputs.tf` (like exports for modules) outputs from the root module just print variables, NOT exports.  Outputs from child modules doesn't print, BUT export.
- `variables.tf` (like arguments) Variables are passed from the root module to child modules.
- `terraform.tfvars` (**only for the root module** and then the root passes the vars to its child modules)
