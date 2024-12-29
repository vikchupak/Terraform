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
