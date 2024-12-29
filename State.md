
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

Implicitly refreshes the state. **On first execution `terraform.tfstate` is created.**
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

# Output state values

```htc
# Only one output value allowed per output
output "dev-vpc-id" {
   value = aws_vpc.development-vpc.id
}
output "dev-subnet-id" {
   value = aws_vpc.dev-subnet-1.id
}
```

# So terraform doesn't see or manage not imported resources?

Correct! Terraform does not see or manage resources that have not been explicitly **imported** into its state file (`terraform.tfstate`) or defined in its configuration files (`.tf`). If a resource exists in your real infrastructure but is not part of the Terraform state, Terraform is completely unaware of its existence.

---

### **What Happens to Non-Imported Resources?**

1. **Terraform State and Configuration**:
   - The `terraform.tfstate` file contains a mapping of all the resources Terraform manages.
   - If a resource is not in the state file, Terraform will not manage or track it.

2. **On `terraform plan`**:
   - Terraform will only create or modify resources that are defined in the `.tf` files and mapped in the state file.
   - Any real-world resources not in the state file will be treated as **non-existent** by Terraform.

3. **On `terraform apply`**:
   - Terraform will create new resources as defined in your configuration without recognizing or interacting with the unmanaged existing resources.

4. **On `terraform destroy`**:
   - Terraform will only destroy resources listed in the state file.
   - Any unmanaged resources will remain untouched.

---

### **What If You Define a Resource Without Importing It?**

If you write a `.tf` configuration file for a resource that already exists in your infrastructure but you don't import it into the state, Terraform will try to **create a new resource** rather than manage the existing one. This can lead to duplication or conflicts.

#### Example:

- **Existing EC2 instance** with ID `i-1234567890abcdef0`.
- You define the following configuration:
  ```hcl
  resource "aws_instance" "example" {
    ami           = "ami-12345678"
    instance_type = "t2.micro"
  }
  ```

- **What happens?**
  - If you do not import the existing EC2 instance using `terraform import`, Terraform will create a new EC2 instance because it doesn't know the existing one already exists.

---

### **How to Avoid Managing Unimported Resources**

1. **Import the Resource**:
   - Use `terraform import` to bring the resource into the Terraform state file:
     ```bash
     terraform import aws_instance.example i-1234567890abcdef0
     ```

2. **Write Accurate Configurations**:
   - Ensure the `.tf` file accurately reflects the existing resource's attributes.

3. **Run `terraform plan`**:
   - Verify that Terraform sees the imported resource and doesn't propose creating a duplicate.

4. **Gradual Transition**:
   - If you have many unmanaged resources, start by importing the most critical ones and gradually expand Terraform's coverage.

---

### **What If You Don’t Want to Manage Certain Resources?**

If there are resources you don’t want Terraform to manage:

1. **Exclude Them from the Configuration**:
   - Do not define those resources in your `.tf` files.

2. **Ensure the State is Not Affected**:
   - Terraform won't interact with resources that are not in its configuration or state file.

---

### **Best Practices**

1. **Always Import Before Managing Existing Resources**:
   - This ensures Terraform doesn't create duplicates.

2. **Review the Plan**:
   - Carefully review the output of `terraform plan` to ensure no unintended actions are proposed.

3. **Iterative Approach**:
   - Import and manage resources incrementally to minimize disruptions.

4. **Use Terraform Import Tools**:
   - Tools like [Terraformer](https://github.com/GoogleCloudPlatform/terraformer) can help automate the import process for large infrastructures.
