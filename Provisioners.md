# Provisioners vs user_data

- `user_data` is managed by a cloud provider. Terraform just passes data to the cloud provider.
- terraform provisioner `remote-exec` is managed by terraform. It ssh to the ec2 instans and runs commands/scripts.
  - If provisioner failes, the whole resource is marked fo delete
 
Avoid using `user_data` or provisioners. Use config management tools like Ansible or others.
