# Terraform Conditional Expressions

Terraform allows you to add conditional logic to your infrastructure code using **conditional expressions**. This helps make your configurations more flexible and dynamic based on certain conditions.

## 1. Conditional Expressions

A conditional expression in Terraform has the following syntax:

```hcl
condition ? true_value : false_value
```

- **condition**: The condition to evaluate (it can be any expression that returns a boolean).
- **true_value**: The value returned if the condition is true.
- **false_value**: The value returned if the condition is false.

## 2. Example of Conditional Expression

Suppose you want to create an EC2 instance only if a certain environment variable (`var.deploy_prod`) is set to `true`. Otherwise, create a smaller instance.

```hcl
resource "aws_instance" "example" {
  ami           = "ami-12345678"
  instance_type = var.deploy_prod ? "t3.large" : "t3.micro"
}
```

In this example:
- If `var.deploy_prod` is `true`, it will provision a `t3.large` instance.
- If `var.deploy_prod` is `false`, it will provision a smaller `t3.micro` instance.

## Real-World Use Cases

### 1. Creating Resources Conditionally

Sometimes you only want to create certain resources under specific conditions, such as provisioning a VPC only in a production environment.

```hcl
resource "aws_vpc" "main" {
  cidr_block = var.is_production ? "10.0.0.0/16" : "192.168.0.0/16"
  enable_dns_support = true
}
```

### 2. Variable Assignments

You can use conditionals to set values of variables based on environment settings or other conditions.

```hcl
variable "environment" {
  type    = string
  default = "dev"
}

output "instance_size" {
  value = var.environment == "prod" ? "t3.large" : "t3.micro"
}
```

## Benefits of Using Conditions

- **Flexibility**: Adjust infrastructure based on dynamic parameters like environment, region, or other configurations.
- **Cleaner Code**: Avoid writing multiple `if` blocks or separate resources for similar configurations.
- **Simplified Management**: Handle conditional logic directly in resource definitions, improving readability.

## Conclusion

Terraform's conditional expressions are powerful tools to customize resource configurations based on different criteria. They make your infrastructure code adaptable to various conditions, allowing you to manage complex environments efficiently.
