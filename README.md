### Project By Adedoyin Ekong


#### **Infrastructure Provisioning with Terraform on GitHub (GitOps & DevSecOps)**

Welcome to my journey in creating the **Infrastructure Provisioning with Terraform on GitHub** project! As someone deeply passionate about DevOps and cloud infrastructure, I set out to build a repository that combines Terraform configurations with GitHub Actions workflows, integrated with comprehensive security tests. By adhering to GitOps principles, this project ensures that all infrastructure changes are automated, version-controlled, and reliable while following the best practices of DevSecOps. I'm excited to share the insights and learnings from this experience with you!

### Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Repository Structure](#repository-structure)
4. [GitHub Actions Workflows](#github-actions-workflows)
5. [Terraform Configuration](#terraform-configuration)
6. [Usage](#usage)
7. [Manual Trigger](#manual-trigger)

### Overview

In this project, I utilized Terraform to define and provision infrastructure on AWS. The deployment process is managed through GitHub Actions workflows, which include steps for initialization, validation, security scanning, planning, and applying changes. This setup ensures that all infrastructure modifications are thoroughly tested before being applied, providing a robust framework for managing cloud resources efficiently and securely.

### Prerequisites

Before you dive in, make sure you have the following:

- An AWS account with IAM user credentials (access key and secret key).
- A GitHub repository with the necessary secrets configured for AWS credentials and other environment variables.

### Repository Structure

This repository is structured to keep everything organized and efficient. Here’s a breakdown of the main components:

- **`.github/workflows/github-ci.yml`:** This is the GitHub Actions workflow file that automates the CI/CD pipeline for Terraform.
- **`main.tf`:** The primary Terraform configuration file where I define the core infrastructure components.
- **`variables.tf`:** This file defines input variables for Terraform, allowing for flexibility and customization.
- **`outputs.tf`:** A file that specifies the outputs of the Terraform configuration, providing useful information after deployment.
- **Other Terraform files:** Additional configuration files to define various AWS resources.

### GitHub Actions Workflows

A significant part of this project was designing a GitHub Actions workflow that automates the deployment process. The workflow file (`.github/workflows/github-ci.yml`) includes several jobs that ensure each step is meticulously handled:

- **Terraform Init:** Initializes Terraform and sets up the backend for state management.
- **Terraform Validate:** Checks the Terraform configuration for syntax errors and other issues.
- **tfsec Security Scan:** Conducts security scans on the Terraform code using tfsec, identifying potential security vulnerabilities.
- **Gitleaks Secret Scan:** Scans the codebase for hardcoded secrets to prevent sensitive data leaks.
- **njsscan:** Performs a security scan specifically targeting Node.js vulnerabilities.
- **Semgrep:** Runs static analysis to detect security vulnerabilities and code quality issues.
- **Retire.js:** Checks for known vulnerabilities in JavaScript libraries.
- **Terraform Plan:** Creates an execution plan, detailing what changes will be made.
- **Terraform Apply:** Applies the changes to the infrastructure, which can be triggered automatically or manually based on the workflow setup.

### Automatic and Manual Deployment

I designed the workflow to support both automatic and manual deployments:

- **Automatic Deployment:** After a successful Terraform plan, the apply job automatically provisions the infrastructure.
- **Manual Deployment:** You can manually trigger the apply job using the `workflow_dispatch` event in GitHub Actions, allowing for more control over the deployment process.

### Terraform Configuration

This project utilizes specific versions and modules to ensure consistency and reliability:

- **Providers:**  
  `hashicorp/aws = 5.3`

- **Modules:**  
  - `terraform-aws-modules/ec2-instance/aws = 5.2.1`  
  - `terraform-aws-modules/vpc/aws = 5.1.0`

**Resources Created:**

- **Role "app-server-role"** with policies:
  - `AmazonSSMManagedInstanceCore`
  - `AmazonEC2ContainerRegistryFullAccess`

- **Role "gitlab-runner-role"** with policies:
  - `AmazonSSMFullAccess`
  - `AmazonEC2ContainerRegistryFullAccess`

- **VPC "main"** and associated security groups:
  - Security group **"main"**
  - Security group **"app-server"**

- **EC2 Instances:**
  - **EC2 server "ec2_app_server"** with:
    - Role: `app-server-role`
    - Security Group: `app-server`
  - **EC2 server "ec2_gitlab_runner"** with:
    - Role: `gitlab-runner-role`
    - Security Group: `main`
  
  *(Note: Both servers use the Ubuntu image: `ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-*`)*

### Usage

To use this setup, you'll need to create a `terraform.tfvars` file with your specific configurations. This file should include the following variables:

- `aws_access_key_id`
- `aws_secret_access_key`
- `aws_region`
- `env_prefix`
- `runner_registration_token`

**Important Notes:**

- `variables.tf` declares all variables used in the script and should be part of your Terraform configuration.
- `terraform.tfvars` assigns values to those variables, including any sensitive data, and should not be committed to your repository.

**Example Format for `terraform.tfvars`:**
```hcl
my_var_one = "value-one"
my_var_two = "value-two"
```

**Terraform Commands:**

- **Initialize project & download providers:**
  ```bash
  terraform init
  ```
- **Preview what will be created with apply & check for errors:**
  ```bash
  terraform plan
  ```
- **Execute with preview:**
  ```bash
  terraform apply -var-file=terraform.tfvars
  ```
- **Execute without preview:**
  ```bash
  terraform apply -var-file=terraform.tfvars -auto-approve
  ```
- **Destroy everything:**
  ```bash
  terraform destroy
  ```
- **Show resources and components from current state:**
  ```bash
  terraform state list
  ```

### Conclusion

This project represents my exploration into automating infrastructure provisioning while maintaining a strong focus on security through DevSecOps practices. By integrating various security tools and leveraging a CI/CD pipeline, I've created a framework that ensures both efficient provisioning and rigorous security standards. I hope my journey inspires you to explore the powerful combination of GitOps and DevSecOps!

### Images

For a visual representation of the architecture and the outcomes of the various security scans, please check out the **IMAGES** directory within this repository.

---

Thank you for following along with my journey in creating this project! I look forward to hearing your feedback and hope this repository helps you manage and secure your AWS infrastructure using GitOps principles.

**Project Created by Adedoyin Ekong - DevSecOps Enthusiast**
