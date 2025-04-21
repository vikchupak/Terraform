- Provisioners are for app config/management, which is, in general, against terraform philosophy.
- Provisioners are last resort.
- [Example](https://gitlab.com/twn-devops-bootcamp/latest/12-terraform/terraform-learn/-/blob/feature/provisioners/main.tf?ref_type=heads#L132)

# Provisioners
- `local-exec`
  - Executes a command or script on the machine running Terraform (your local workstation or the Terraform host).
  - Runs locally where Terraform is executed.
  - When you need to perform actions locally, like triggering a script, sending notifications, or invoking APIs after a resource is created.
  - **Executes after resource creation.**
- `remote-exec`
  - Executes a command or script on the target resource (e.g., a virtual machine) via SSH or WinRM.
  - Requires a connection to the target resource (e.g., via SSH).
  - When you need to configure a newly created instance by running remote scripts or commands.
  - **Executes after resource creation.**
- `file`
  - Transfers files or directories from the local machine to a target resource (e.g., virtual machines or instances).
  - Requires a connection setup, similar to remote-exec.
  - When you need to copy configuration files, scripts, or other resources to a newly created instance.
  - **Executes after resource creation.**
- `user_data`
  - Passes a script or cloud-init configuration to an instance when it boots. **This is not a Terraform provisioner** but is commonly used with resources like aws_instance.
  - When you want to configure an instance during its initialization process (e.g., installing software, setting up services, or running scripts).
  - **Runs at instance startup as part of the bootstrapping process.**
  - Does not require SSH or manual interaction; the cloud provider handles execution.
  - Often used in combination with cloud-init for more complex configurations.

# Provisioners vs user_data

- `user_data` is managed by a cloud provider. Terraform just passes data to the cloud provider.
- terraform provisioner `remote-exec` is managed by terraform. It ssh to the ec2 instans and runs commands/scripts.
  - If provisioner failes, the whole resource is marked fo delete
 
Avoid using `user_data` or provisioners. Use config management tools like Ansible or others.

# null_resource

Provisioneres are run after resource creation. To run provisioner without resource creation, we can use `null_resource`

```hcl
resource "null_resource" "configure_server" {
  triggers = {
    trigger = aws_instance.myapp_server.public_ip
  }

  provisioner "local-exec" {
    working_dir = "~/ansible"
    command = "ansible-playbook --inventory ${aws_instance.myapp_server.public_ip}, --private-key ${var.ssh_private_key} --user ec2-user ansible-play-book.yaml"
  }
}
```

