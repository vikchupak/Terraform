# Terraform variables

```hcl
...

variable "subnet_cidr_block" {
  description = "subnet cidr block"
  default = "10.0.10.0/24" # to make the variable optional, no prompt will appear
  type = string # enforce type constraint
  # type = list(string)
  # type = list(object{
  #  cidr_block = string
  #  name = string
  # })
}

resource "aws_subnet" "terraform-subnet" {
  vpc_id = aws_vpc.terraform-vpc.id
  cidr_block = var.subnet_cidr_block
  # cidr_block = var.subnet_cidr_block[0].cidr_block
  availability_zone = "eu-central-1a"
}
```

3 ways to pass value to input

- Run `terraform apply` and we will get a prompt to type the value.
- `terraform apply -var "<var-name>=<var-value>"`
- Use `terraform.tfvars` file. Terraform automatically finds this file and implicitly applies when `terraform apply`
  - To use differently named `.tfvars` file, run `terraform apply -var-file <file-name>.tfvars`

# Env variables

## Use `already pre-configured/pre-declared` ENV vars. Like Provider-Specific ENVs. No `TF_VAR_` prefix needed.

https://registry.terraform.io/providers/taiidani/jenkins/latest/docs

```hcl
# Configure the Jenkins Provider
provider "jenkins" {
  server_url = "https://jenkins.url" # Or use JENKINS_URL env var
  username   = "username"            # Or use JENKINS_USERNAME env var
  password   = "password"            # Or use JENKINS_PASSWORD env var
  ca_cert = ""                       # Or use JENKINS_CA_CERT env var
}

# Create a Jenkins job
resource "jenkins_job" "example" {
  # ...
}
```

## Use custom ENV vars

Using `TF_VAR_` env prefix to map the envs to terraform input variables. Without `TF_VAR_` terraform does not automatically associate them with variables in your configuration(so default value will be used if provided).

```bash
export TF_VAR_avil_zone="some-zone"
```

```hcl
variable avil_zone {
   default = "us-east-1"
}

resource "aws_subnet" "terraform-subnet" {
  vpc_id = aws_vpc.terraform-vpc.id
  availability_zone = var.avil_zone
}
```

# String interpolation

```hcl
variable "environment" {
  default = "prod"
}

resource "aws_instance" "example" {
  tags = {
    Name = "${var.environment}-web-server" # String interpolation syntax
  }
}
```
