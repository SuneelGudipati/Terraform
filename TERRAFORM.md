## Terraform Project Documentation

### Overview

This project uses [Terraform](https://www.terraform.io/) to manage infrastructure resources in a declarative manner. The infrastructure is modularized to allow easy reuse across different environments. This repository demonstrates how to provision cloud resources (e.g., AWS EC2 instances) and configure them using Terraform.

### Project Structure

The project is organized as follows:

```
terraform-example/
├── modules/                # Reusable Terraform modules
│   └── server/             # Server module for provisioning EC2 instances
│       ├── main.tf         # Server resource definition
│       ├── variables.tf    # Variables for the server module
│       └── outputs.tf      # Outputs for the server module
├── environment/            # Environment-specific configuration (e.g., dev, prod)
│   ├── dev.tf              # Development environment configuration
│   ├── prod.tf             # Production environment configuration
│   └── variables.tf        # Common environment variables
├── main.tf                 # Root configuration that calls the server module
├── provider.tf             # Provider configuration (e.g., AWS)
└── README.md               # Project documentation (this file)
```

- **`modules/`**: Contains reusable modules. For example, the `server` module provisions an EC2 instance.
- **`environment/`**: Contains environment-specific configuration files. These files can be tailored for specific environments (e.g., `dev`, `prod`).
- **`main.tf`**: The root configuration that brings together all modules and resources.
- **`provider.tf`**: Contains the provider configuration (e.g., AWS, Azure).
- **`README.md`**: The documentation for the project.

### Prerequisites

Before running the Terraform code, ensure the following:

- **Terraform** is installed on your machine. You can download it from [here](https://www.terraform.io/downloads.html).
- **AWS CLI** is installed and configured with your AWS credentials. You can configure the CLI using `aws configure`.
- **An AWS account** or a cloud provider account if you are using another provider.

### Usage

#### 1. Clone the repository

```bash
git clone https://github.com/yourusername/terraform-example.git
cd terraform-example
```

#### 2. Configure your provider

If you're using AWS, configure your AWS credentials using the AWS CLI or set up environment variables:

```bash
aws configure
```

Alternatively, you can configure the provider directly in the `provider.tf` file (not recommended for production).

#### 3. Initialize Terraform

Run `terraform init` to initialize the working directory containing Terraform configuration files. This will download necessary provider plugins and initialize the backend if used.

```bash
terraform init
```

#### 4. Plan the changes

You can run `terraform plan` to see which resources will be created, modified, or destroyed before actually applying them.

```bash
terraform plan -out=tfplan
```

#### 5. Apply the configuration

After reviewing the plan, you can apply it to create the resources:

```bash
terraform apply tfplan
```

Alternatively, you can apply directly without a plan file:

```bash
terraform apply
```

You will be prompted to confirm the action before resources are created.

#### 6. Managing different environments

The project supports multiple environments (e.g., dev, prod). You can apply configurations specific to each environment.

To apply for **development**:

```bash
terraform apply -var-file="environment/dev.tfvars"
```

To apply for **production**:

```bash
terraform apply -var-file="environment/prod.tfvars"
```

#### 7. Destroy the infrastructure

If you want to tear down the infrastructure, use the `terraform destroy` command:

```bash
terraform destroy
```

You will be prompted to confirm the destruction of all resources.

### Modules

#### `server` Module

This module provisions an EC2 instance in AWS. It is reusable across different environments.

- **Inputs**:
  - `ami_id` (string): The AMI ID for the EC2 instance.
  - `instance_type` (string): The instance type (e.g., `t2.micro`).
  - `instance_name` (string): The name tag for the instance.

- **Outputs**:
  - `instance_id`: The ID of the created EC2 instance.
  - `instance_public_ip`: The public IP address of the instance.

##### Example Usage in `dev.tf`:

```hcl
module "server" {
  source        = "../modules/server"
  ami_id        = "ami-12345678"  # Replace with your dev AMI ID
  instance_type = "t2.micro"
  instance_name = "dev-server"
}
```

### Variables

#### `variables.tf`

This file contains variables for configuring the project. Example variables include `ami_id`, `instance_type`, etc.

You can define environment-specific variables in `environment/dev.tfvars` or `environment/prod.tfvars` files. These files override the default values in `variables.tf`.

#### Example `variables.tf`:

```hcl
variable "ami_id" {
  description = "AMI ID for the instance"
  type        = string
}

variable "instance_type" {
  description = "Instance type"
  type        = string
}

variable "instance_name" {
  description = "The name of the instance"
  type        = string
}
```

#### Example `dev.tfvars`:

```hcl
ami_id        = "ami-12345678"
instance_type = "t2.micro"
instance_name = "dev-server"
```

### Outputs

The Terraform configuration outputs useful information once the infrastructure is created. For example, when provisioning a server, you might want to output the instance ID and its public IP:

```hcl
output "instance_id" {
  value = aws_instance.server.id
}

output "instance_public_ip" {
  value = aws_instance.server.public_ip
}
```

### Best Practices

- **State Management**: For collaborative work, it is a good idea to use a remote backend (e.g., S3 + DynamoDB for locking) to store the Terraform state.
- **Version Control**: Store your Terraform code in version control systems like Git. However, **do not store sensitive data** like AWS keys or secret tokens in the repository.
- **Environment-Specific Configurations**: Use different variable files (e.g., `dev.tfvars`, `prod.tfvars`) to manage different environments.
- **Modularize Resources**: Use Terraform modules to keep the configuration DRY (Don't Repeat Yourself) and make it reusable across multiple environments.

### Troubleshooting

- **Insufficient permissions**: If Terraform encounters errors related to insufficient permissions, ensure that the IAM role or user has the appropriate permissions to create, read, modify, and delete the resources in question.
- **State Locking**: If you're using a remote backend (e.g., S3), ensure that DynamoDB is configured for state locking to prevent race conditions.
