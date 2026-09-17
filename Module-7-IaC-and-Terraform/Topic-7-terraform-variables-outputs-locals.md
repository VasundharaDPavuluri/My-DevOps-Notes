# Terraform Variables, Outputs & Locals: Making Infrastructure Reusable

## Overview

We’ve seen how Terraform Modules help us reuse infrastructure.

But another question comes up:

How do we make the same Terraform code work for different environments?

Dev may need one instance type.

Test may need another.

Production may need a different one.

We don’t want to rewrite the infrastructure code every time.

This is where **Variables, Outputs and Locals** become important.

---

## 1. Terraform Variables

Variables allow us to pass values into Terraform instead of hardcoding them.

For example:

```hcl
variable "instance_type" {
  type    = string
  default = "t2.micro"
}
```

We can then use the variable in a resource:

```hcl
resource "aws_instance" "web" {
  instance_type = var.instance_type
}
```

The same Terraform configuration can now work with different values:

```text
Dev  → t2.micro
Test → t3.micro
Prod → t3.medium
```

### Variables = Inputs to Terraform

---

## 2. Terraform Outputs

Sometimes we need to expose information after Terraform creates infrastructure.

For example:

```hcl
output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

After `terraform apply`, Terraform can display the instance IP.

Outputs are also useful when one module needs to provide information to another module.

### Outputs = Information Exposed by Terraform

---

## 3. Terraform Locals

Locals allow us to define reusable values inside a Terraform configuration.

For example:

```hcl
locals {
  environment = "production"
  name        = "web-${local.environment}"
}
```

We can then reuse these values:

```hcl
tags = {
  Name        = local.name
  Environment = local.environment
}
```

Instead of repeating the same expressions throughout the configuration.

### Locals = Internal Reusable Values

---

## 4. How They Work Together

A simple way to understand them:

```text
Variables
    ↓
Input Configuration
    ↓
Terraform Resources
    ↓
Outputs
```

Locals support reusable values within the configuration.

Another simple way to remember:

```text
Variables → What goes IN

Locals → What we reuse INSIDE

Outputs → What comes OUT
```

---

## 5. Why This Matters at Scale

Consider the same infrastructure across:

```text
Dev → Test → Staging → Production
```

The infrastructure logic can remain the same while variables provide environment-specific values.

This gives us:

- Reusability
- Consistency
- Less duplication
- Easier maintenance
- Cleaner configurations

---

## 6. Separating Logic from Configuration

Variables, Outputs and Locals are not just Terraform syntax.

They help us separate:

```text
Infrastructure Logic
        from
Configuration Values
```

For example:

```text
Infrastructure Logic
        ↓
Terraform Resources

Environment Values
        ↓
Variables

Reusable Internal Values
        ↓
Locals

Exposed Information
        ↓
Outputs
```

This separation makes Terraform configurations easier to reuse and maintain.

---

## Architect-Level Perspective

Good Terraform code should avoid unnecessary hardcoding.

Instead:

```text
Variables → Configuration
Locals    → Internal Logic
Outputs   → Exposed Information
Modules   → Reusable Infrastructure
```

When combined with Modules, these concepts allow us to build infrastructure that is reusable across multiple environments without becoming tightly coupled to a single environment.

---

## Key Takeaways

- **Variables** provide inputs to Terraform.
- **Locals** define reusable values inside the configuration.
- **Outputs** expose information from Terraform resources or modules.
- Variables help avoid unnecessary hardcoding.
- Locals help reduce repetition.
- Outputs help pass information between modules and expose useful values.
- Together with Modules, they help create reusable and maintainable Terraform configurations.

---

## Simple Way to Remember

```text
Variable → Input

Local → Internal Value

Output → Result

Module → Reusable Infrastructure
```

Together, these concepts form the foundation for writing maintainable Terraform configurations.

-----

<img width="624" height="836" alt="Terraform Variables, Outputs   Locals" src="https://github.com/user-attachments/assets/af895669-7c68-46ae-bf4b-dea0bf079436" />
