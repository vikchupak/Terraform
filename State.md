
- After each apply/destroy command, terraform creates/updates `terraform.tfstate` with your real resources state.
- `terraform.tfstate.backup` - previous state.

Refresh/actualize state
```bash
terraform refresh
```

Implicitly refreshes the state. if `terraform.tfstate` was already created. **Without a state file, terraform assumes that all resources in the configuration need to be created.**
```bash
terraform plan
```

Implicitly refreshes the state. **On first execution `terraform.tfstate` is created.**\
```bash
terraform apply
```

- To add to state already existing resources
- https://github.com/GoogleCloudPlatform/terraformer (Infrastructure to Code, reverse Terraform)
```
terraform import aws_instance.example i-1234567890abcdef0
```

List all resources
```bash
terraform state list
```

Show resource state
```bash
terraform state show <resource-name>
```
