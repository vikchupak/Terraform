# Project structure

- https://developer.hashicorp.com/terraform/language/style#file-names

Here's a typical **Terraform project structure** and common files:

```
terraform-project/
├── provider.tf        # Provider(s) configuration (AWS, Azure, etc.)
├── main.tf            # Main resources definition
├── variables.tf       # Input variables
├── terraform.tfvars   # Values for variables (gitignore sensitive ones)
├── outputs.tf         # Output values
├── versions.tf        # Terraform and provider version constraints
├── backend.tf         # Remote backend configuration (optional)
├── modules/           # Reusable child modules (if used)
│   └── <module-name>/
│       ├── main.tf
│       ├── variables.tf
│       ├── outputs.tf
├── environments/      # Separate environments (dev, prod, staging)
│   └── dev/
│       ├── main.tf
│       ├── terraform.tfvars
│   └── prod/
│       ├── main.tf
│       ├── terraform.tfvars
├── scripts/           # Any helper scripts (bash, python, etc.)
├── README.md          # Project documentation
└── .terraform/        # Terraform's local cache (auto-generated, ignored)
```

Terraform does not require you to explicitly import or include `.tf` files.
- **Terraform automatically loads all `.tf` files** **in the same directory**.
- It **merges** them together **at runtime** as if they were one big file.

**The only exception** is when you work with modules (inside `./modules/` folder) — there you do need to reference them explicitly, like this:
- But that's for reusable components, not normal project files.
  ```hcl
  module "example" {
    source = "./modules/example"
    variable1 = "value1"
  }
  ```

![image](https://github.com/user-attachments/assets/6937ee21-0176-4149-868a-9902a66bd2b8)

---

### Quick overview of the **main files**:

| File | Purpose |
|:-----|:--------|
| `provider.tf` | Provider setup (AWS, Azure, GCP, etc.) |
| `main.tf` | Core resource definitions |
| `variables.tf` | Declares inputs that the module/project needs |
| `terraform.tfvars` | Auto-loaded file with variable values |
| `outputs.tf` | Exposes outputs after apply |
| `versions.tf` | Lock Terraform CLI and providers' versions |
| `backend.tf` | Configure remote state (e.g., S3, Terraform Cloud) |

---

### Pro tips:
- Always **`gitignore`** `.terraform/`, `*.tfstate`, and `*.tfstate.backup`.
- You can **split `.tf` files** however you want — Terraform loads them all in the same folder.
- Keeping environments separated (e.g., `environments/dev`, `environments/prod`) is a good practice when your infrastructure grows.
