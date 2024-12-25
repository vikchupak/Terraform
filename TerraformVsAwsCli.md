`Terraform` and `AWS CLI` are both tools used to manage AWS resources, but they serve different purposes and operate in distinct ways.

Terraform is specifically for provisioning infrastracture:
 - Create VPC
 - Create AWS users and permissions
 - Spin up servers
 - *Install Docker, etc. Installing Docker is more about **configuring or preparing the software environment** on an existing resource. Ansible, Chef, or scripts in Terraform can handle this type of post-provisioning configuration. But it is more configuring rather than provisioning infrastracture.

**AWS CLI can be used for provisioning infrastructure, but its purpose extends far beyond that.**

Limitations of Using AWS CLI for Provisioning.\
While AWS CLI can provision infrastructure, it has significant limitations compared to tools like **Terraform**:

1. **Imperative Workflow**:
   - AWS CLI uses an **imperative approach**, meaning you explicitly define the steps to create or modify resources.
   - You are responsible for manually ensuring dependencies and relationships between resources.

2. **No State Management**:
   - The AWS CLI does not maintain a record of what resources you’ve created or their current state.
   - If you accidentally rerun commands, resources may be duplicated or misconfigured.

3. **Complexity**:
   - For large or interconnected infrastructures (e.g., VPCs, subnets, EC2 instances, and RDS databases), managing everything manually with CLI commands becomes error-prone and difficult to maintain.

4. **Lack of Version Control**:
   - CLI commands are not as easily versioned or reusable as Infrastructure-as-Code (IaC) files written in Terraform or CloudFormation.

## Provisioning vs Configuration
  - **Provisioning**: Setting up the infrastructure itself.
  - **Configuration**: Installing and setting up the software on provisioned resources.
  - Tools like **Terraform** (provisioning) and **Ansible** (configuration) often complement each other.

## Terraform vs AWS CLI

- Terraform
  - A **declarative Infrastructure-as-Code (IaC)** tool.
  - You describe the desired state of your infrastructure in configuration files, and Terraform ensures your AWS resources match that state.
  - Terraform tracks the state of your infrastructure in a state file.
- AWS CLI
  - A command-line interface for interacting with AWS services.
  - Used for executing specific **imperative commands** to manage AWS resources (e.g., create an S3 bucket, start an EC2 instance).
  - No concept of maintaining state - each command operates independently.
