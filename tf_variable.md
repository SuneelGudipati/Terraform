
# Terraform Variables Documentation

In Terraform, variables are used to manage dynamic configurations, making your code more reusable and flexible. This documentation provides an overview of Terraform variables, including their types, how to declare and assign values, variable precedence, and handling sensitive data.

## Types of Variables

Terraform supports several types of variables to manage configurations dynamically:

1. **Input Variables**  
   Defined in `.tf` files, input variables allow you to pass dynamic values to your configuration. These can be set with default values or passed at runtime.

2. **Environment Variables**  
   You can use environment variables to set values directly using the `TF_VAR_` prefix.  
   Example: `TF_VAR_variable_name=value`

3. **Output Variables**  
   Output variables are defined using `output` blocks and are used to export values from one module to another or display output after `terraform apply`.  
   Example:
   ```hcl
   output "instance_ip" {
     value = aws_instance.my_instance.public_ip
   }
   ```

---

## Declaring Variables

Variables are declared using the `variable` block in `.tf` files. Here’s an example declaration:

```hcl
variable "name_of_the_variable" {
  type        = data_type
  description = "description"
  default     = "value"
}
```

In the example above:
- `type`: Specifies the data type of the variable (e.g., `string`, `number`).
- `description`: Provides a short description of the variable.
- `default`: Sets the default value for the variable, which can be overridden later.

---

## Types of Variable Values

Terraform supports different types for variable values:

- **String**: Holds textual data.
- **Number**: For numerical values.
- **Bool**: Boolean values (`true` or `false`).
- **Complex Types**: Lists, maps, and objects for structured data.
---

## Assigning Values

You can assign values to variables in several ways:

1. **Default Values**  
   You can assign default values directly in the variable declaration block (as shown in the example above).

2. **Command-Line Flags**  
   Use the `-var` flag to pass a value when running Terraform commands.  
   Example:  
   ```bash
   terraform apply -var="instance_type=t2.large"
   ```

3. **Files**  
   You can define values in `.tfvars` files and pass them using the `-var-file` flag. Example:
   ```bash
   terraform apply -var-file="production.tfvars"
   ```

---

## Variable Precedence

Terraform evaluates variable values in a specific order of precedence:

1. **Command-line Flags** (`-var`)
2. **`.tfvars` Files** (e.g., `terraform.tfvars`, `prod.tfvars`)
3. **Environment Variables** (e.g., `TF_VAR_variable_name=value`)
4. **Default Values** (defined in the variable block)

This means that if the same variable is set in multiple places, Terraform will prioritize the values in the order listed above.

---

## Sensitive Variables

When dealing with sensitive data (like passwords, API keys, etc.), you can set the `sensitive` argument to `true` in the variable declaration. This prevents Terraform from displaying the value in output or logs.

Example:
```hcl
variable "db_password" {
  type      = string
  sensitive = true
}
```

This ensures that sensitive values are masked when outputting logs or running commands like `terraform plan` or `terraform apply`.

---

## Conclusion

Using variables in Terraform makes configurations more adaptable, enabling you to change infrastructure parameters without modifying the core code. You can declare variables, pass dynamic values, handle different types of data, and manage sensitive values securely to ensure flexibility and scalability in your infrastructure as code.
