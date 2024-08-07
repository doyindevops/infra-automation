## README.md

### Infrastructure Provisioning with Terraform on GitHub (GitOps & DevSecOps)

This repository contains Terraform configurations and GitHub Actions workflows integrated with comprehensive security tests, used to provision and manage AWS infrastructure following GitOps principles. This setup ensures that all infrastructure changes are automated, version-controlled, and reliable, adhering to the highest standards of DevSecOps practices.

---

### Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Repository Structure](#repository-structure)
4. [GitHub Actions Workflows](#github-actions-workflows)
5. [Terraform Configuration](#terraform-configuration)
6. [Usage](#usage)
7. [Manual Trigger](#manual-trigger)


---

### Overview

This project leverages Terraform to define and provision infrastructure on AWS. The deployment process is managed by GitHub Actions workflows, which include steps for initialization, validation, security scanning, planning, and applying the changes. This approach ensures that all infrastructure changes are thoroughly tested before being applied.

### Prerequisites

Before you begin, ensure you have the following:
- An AWS account with IAM user credentials (access key and secret key).
- A GitHub repository with the necessary secrets configured for AWS credentials and other environment variables.

### Repository Structure

The repository contains the following main components:
- **.github/workflows/github-ci.yml**: GitHub Actions workflow file that automates the CI/CD pipeline for Terraform.
- **main.tf**: Main Terraform configuration file.
- **variables.tf**: File defining input variables for Terraform.
- **outputs.tf**: File defining the outputs of the Terraform configuration.
- **Other Terraform files**: Additional configuration files that define various AWS resources.

### GitHub Actions Workflows

The GitHub Actions workflow (`.github/workflows/github-ci.yml`) automates the entire deployment process, including the following jobs:
- **Terraform Init**: Initializes Terraform and sets up the backend.
- **Terraform Validate**: Validates the Terraform configuration to ensure it is syntactically correct.
- **tfsec Security Scan**: Runs security scans on the Terraform code using tfsec.
- **Gitleaks Secret Scan**: Scans for hardcoded secrets in the codebase.
- **njsscan**: Performs a security scan specifically for Node.js vulnerabilities.
- **Semgrep**: Runs static analysis to find security vulnerabilities and code quality issues.
- **Retire.js**: Checks for known vulnerabilities in JavaScript libraries.
- **Terraform Plan**: Creates an execution plan for the changes, showing what will be added, changed, or destroyed.
- **Terraform Apply**: Applies the changes to the infrastructure. This job can run automatically after a successful plan or be manually triggered.

### Automatic and Manual Deployment

The workflow supports both automatic and manual deployments:
- **Automatic Deployment**: After a successful Terraform plan, the apply job runs automatically to provision the infrastructure.
- **Manual Deployment**: You can manually trigger the apply job using the `workflow_dispatch` event in GitHub Actions.

### Terraform Configuration

#### Versions Used in the Project

**Providers**
- `hashicorp/aws = 5.3`

**Modules**
- `terraform-aws-modules/ec2-instance/aws = 5.2.1`
- `terraform-aws-modules/vpc/aws = 5.1.0`

#### Resources Created

- **Role "app-server-role"** with policies:
  - AmazonSSMManagedInstanceCore
  - AmazonEC2ContainerRegistryFullAccess
- **Role "gitlab-runner-role"** with policies:
  - AmazonSSMFullAccess
  - AmazonEC2ContainerRegistryFullAccess
- **VPC "main"**
- **Security group "main"**
- **Security group "app-server"**
- **EC2 server "ec2_app_server"** with:
  - Role: app-server-role
  - Security Group: app-server
- **EC2 server "ec2_gitlab_runner"** with:
  - Role: gitlab-runner-role
  - Security Group: main

*Note*: Both servers use the Ubuntu image: `ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-*`

### Usage

#### Create `terraform.tfvars` File

Before running the script, create a `terraform.tfvars` file and set the following variables:
- `aws_access_key_id`
- `aws_secret_access_key`
- `aws_region`
- `env_prefix`
- `runner_registration_token`

*Notes*:
- `variables.tf` declares all variables used in the script and is a normal part of the Terraform script. 
- `terraform.tfvars` assigns values to those declared variables, including secret variables, so it should be created and used locally, not committed to the repository as part of the code.

Format inside `terraform.tfvars`:
```hcl
my_var_one = "value-one" 
my_var_two = "value-two"
```

#### Terraform Commands

- Initialise project & download providers:
  ```bash
  terraform init
  ```

- Preview what will be created with apply & see if any errors:
  ```bash
  terraform plan
  ```

- Execute with preview:
  ```bash
  terraform apply -var-file=terraform.tfvars
  ```

- Execute without preview:
  ```bash
  terraform apply -var-file=terraform.tfvars -auto-approve
  ```

- Destroy everything:
  ```bash
  terraform destroy
  ```

- Show resources and components from current state:
  ```bash
  terraform state list
  ```

### Conclusion

This setup demonstrates a professional DevSecOps approach, ensuring that infrastructure is not only provisioned efficiently but also adheres to security best practices. The integration with various security tools and the use of a CI/CD pipeline highlight the commitment to maintaining a secure and robust infrastructure.

---

### IMAGES


---

By following the instructions and utilizing the provided workflows and Terraform configurations, you can effectively manage and secure your AWS infrastructure using GitOps principles.
